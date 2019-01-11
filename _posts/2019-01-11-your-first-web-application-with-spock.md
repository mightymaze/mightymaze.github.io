---
title: Your First Web Application with Spock
tags:
- haskell
---

The Spock web framework for Haskell gives you a light but complete foundation to build web servers on, be it for traditional server-side rendered applications, or APIs for single-page applications. Compared to the Scotty framework, Spock is slightly richer in its feature set, and is more opinionated regarding the application architecture. In this episode we explore the basics of Spock and in-memory server state, as we build a note keeper application.

##### Show Notes
We are starting with an empty project called notekeeper, and a Cabal file specifying an executable. To get started, we add dependencies for Spock, http-types, text, and mtl.

```cabal
name:                notekeeper
version:             0.1.0.0
category:            Web
build-type:          Simple
cabal-version:       >=1.10

executable notekeeper
  main-is:            Main.hs
  build-depends:      base >=4.10 && <4.11
                    , Spock
                    , http-types
                    , text
                    , mtl
  hs-source-dirs:     src
  default-language:   Haskell2010
```

In `Main.hs`, we start out by importing the `Web.Spock` and `Web.Spock.Config` modules. Those are all we need to get a basic server running.

```haskell
module Main where

import Web.Spock
import Web.Spock.Config
```

The function we need in our definition of `main` is `runSpock`, which has the type:

```haskell
runSpock :: Port -> IO Middleware -> IO ()
```

We run our server on port 8080, with the middleware returned by a function called `spock`. It takes a configuration, and our application. We’ll leave those as typed holes for now, and see what we need.

```haskell
main :: IO ()
main = do
  runSpock 8080 (spock _cfg _app)
```

The `_app` hole tells us we need some value of type `SpockM conn0 sess0 st0 ()`, which is the Spock monad for defining an application. It also tells us that the type variables used as parameters are ambiguous.

```
Found hole: _app :: SpockM conn0 sess0 st0 ()
Where: ‘conn0’ is an ambiguous type variable
       ‘sess0’ is an ambiguous type variable
       ‘st0’ is an ambiguous type variable
```

The `_cfg` hole tells us that we need a value of type `SpockCfg conn0 sess0 st0`, and that the same type variables as we saw before are ambiguous. There is a connection between the type parameters of `SpockM` and `SpockCfg`.

```
Found hole: _cfg :: SpockCfg conn0 sess0 st0
Where: ‘conn0’ is an ambiguous type variable
       ‘sess0’ is an ambiguous type variable
       ‘st0’ is an ambiguous type variable
```


We begin by defining `app` locally, not doing any routing, only returning ().

```haskell
main :: IO ()
main = do
  let app = return ()
  runSpock 8080 (spock _cfg app)
```

This would work, but the type variables are still ambiguous for the configuration. If we move the definition of `app` to the top-level, and give it an explicit type signature, we can pin down those type arguments.

The type `SpockM` is parameterized by four types:

- the database connection type (`conn`)
- the session type (`sess`)
- the state type (`st`)
- the monadic return value’s type

Initially, we won’t have any database, sessions, or state, so we use the () type for all of them.

```haskell
app :: SpockM () () () ()
app = return ()
```

Now that we’ve specified which types we are using in our application, the typed hole `_cfg` has no ambiguous variables. To construct a configuration value, we use the `defaultSpockCfg`, taking a session configuration, a pool or connection for a database, and an initial state. The database parameter cannot be a `()` value, and we’ll leave it as a typed hole to illustrate why.

```haskell
Found hole: _db :: PoolOrConn ()
```

PoolOrConn is parameterized by `()`, and there is only one value with that type: the `PCNoDatabase` constructor defined by Spock. Using that, we obtain a configuration for our Spock application, and we have a rather useless, but working web server.

```haskell
main :: IO ()
main = do
  cfg <- defaultSpockCfg () PCNoDatabase ()
  runSpock 8080 (spock cfg app)
```

To run the web server, and have it reload on source code changes, we can use the excellent `ghcid` tool. By default, it only reloads your code and prints any type errors, but if we override the test command using -T, we can have it run the main function when the code successfully compiles.

```shell
$ ghcid -T :main
Running test...

Spock is running on port 8080
```

In our web browser, we can see that we get a “404 Not Found” when requesting http://localhost:8080. We’ll keep ghcid running in the background.

##### Defining Routes

Before we begin defining routes, we’ll create a type alias for our application type to factor out the `SpockM` details. `Server` takes a single type argument `a`, the monadic return value type. We use it as the type of our `app` definition.

```haskell
type Server a = SpockM () () () a

app :: Server ()
app = return ()
```

We define a route for GET requests to the root path, responding with a text greeting.

```haskell
app :: Server ()
app = get root (text "Hello!")
```

Now we get a type error, explaining that we can’t pass a regular `String` to the `text` function. We enable `OverloadedStrings` to construct a `Text` value instead.

```haskell
{-# LANGUAGE OverloadedStrings #-}
```

Reloading the web browser, we see our greeting “Hello!” displayed.

##### Rendering HTML

Responding with plain text in a web application is a little dull. Instead, we’ll respond with HTML using the html function, and some lovingly hand-crafted markup.

```haskell
app :: Server ()
app = get root (html "<h1>Hello!</h1>")
```

Now get a proper heading, at least.

Constructing HTML markup in strings is not great, though. Instead, we’ll use Lucid, an embedded DSL for HTML markup. In our Cabal file, we add the dependency lucid, along with Spock-lucid, a small library integrating Spock and Lucid.

```cabal
executable notekeeper
  main-is:            Main.hs
  build-depends:      base >=4.10 && <4.11
                    , Spock
                    , http-types
                    , text
                    , mtl
                    , lucid
                    , Spock-lucid
  hs-source-dirs:     src
  default-language:   Haskell2010
```

Back in `Main.hs`, we import the lucid function from `Web.Spock.Lucid`, along with the entire `Lucid` module.

```haskell
import Web.Spock.Lucid (lucid)
import Lucid
```

Now we can use lucid instead of html, and the Lucid DSL to construct our markup. Using do notation, we can define sibling elements, which in our case will be a heading and a paragraph.

```haskell
app :: Server ()
app = get root $ lucid $ do
  h1_ "Hello!"
  p_ "How are you today?"
```

Reloading the web browser, we see the two elements rendered.

##### Keeping Notes with Server State

We are ready to implement the note keeping functionality of our application. Instead of using a server state of type (), we’ll define and use the `ServerState` and `Note` data types.

The server state consists of an IORef holding a list of `Note` values. An `IORef` is a variable that can be mutated atomically in the IO monad.

```haskell
newtype ServerState = ServerState { notes :: IORef [Note] }
```

A note has an author, and contents, both of type `Text`.

```haskell
data Note = Note { author :: Text, contents :: Text }
```

To use Text and IORef we need to import them.

```haskell
import Data.Text (Text)
import Data.IORef
```

Finally, we change the `Server` type alias to use `ServerState`.

```haskel
type Server a = SpockM () () ServerState a
```

Navigating to the next type error we get, we see that we cannot use an application requiring a state of type ServerState with a configuration with state of type ().

```
Couldn't match type ‘ServerState’ with ‘()’
      Expected type: SpockM () () () ()
      Actual type: Server ()
```

We construct a `ServerState` by mapping the constructor over a `newIORef` action, with an empty list of notes, and use it in our configuration.

```haskell
main :: IO ()
main = do
  st <- ServerState <$> newIORef []
  cfg <- defaultSpockCfg () PCNoDatabase st
  runSpock 8080 (spock cfg app)
```

It compiles again! To have something that we can render, we add two notes to the initial state.

```haskell
main :: IO ()
main = do
  st <- ServerState <$>
    newIORef [ Note "Alice" "Must not forget to walk the dog."
             , Note "Bob" "Must. Eat. Pizza!"
             ]
  cfg <- defaultSpockCfg () PCNoDatabase st
  runSpock 8080 (spock cfg app)
```

Now we have something to render, so we change the greeting markup to instead list the notes in our state. We’ll use an unordered list, constructed using `ul_`, and for each note render a list item, containing the note author and contents.

```haskell
app :: Server ()
app = get root $ lucid $ do
  h1_ "Notes"
  ul_ $ forM_ notes' $ \note -> li_ $ do
    toHtml (author note)
    ": "
    toHtml (contents note)
```

The `forM_` function is not in scope, so we need to import it.

```haskell
import Control.Monad (forM_)
```

Also, we don’t have a list of notes available. We need to read it from the IORef in our server state. The getState action in Spock returns our server state, from which we can extract and read the IORef.

```haskell
app :: Server ()
app = get root $ do
  notes' <- getState >>= (liftIO . readIORef . notes)
  lucid $ do
    h1_ "Notes"
    ul_ $ forM_ notes' $ \note -> li_ $ do
      toHtml (author note)
      ": "
      toHtml (contents note)
```

The usage of getState might remind you of ask from the Reader monad.

The readIORef action has to be lifted into the ActionCtxt, which is the monadic type for Spock routes. Thus, we import and use liftIO.

```haskell
import Control.Monad.IO.Class (liftIO)
```

No more errors, so we refresh the web browser and see our list of notes rendered.