---
title: How to call a C function in Golang
tags:
- go
- c
---

I'm writting this article after a very long time. This happens due to the work pressure in my daily life. There is not enough free time to write any article, even customize my own sites / portfolios.

Okay, today I will be showing you guys how to call native C (you can call C++ methods too) functions using Golang. To accomplish this, I would think that you guys have this things installed on your system:

* go
* gcc
* pkg-config

Let's create a project in your `$GOPATH/src`. I have created my project in `$GOPATH/src/github.com/mazhar266/goc`. There I've added a file named `square.c` with the following function:

```c
int square(int a)
{
  return a * a;
}
```

Now we will call this function from our **go** program. Let's create a blank `main.go` file. Let's just start writting the basic structure like:

```go
package main

import "fmt"

func main() {
  ...
}
```

Now include the `C` package and include our `square.c` file which contains our **square** function. Add this line after package declaration:

```go
//#include<square.c>
import "C"
```

This first line imports our **C** program (the `square.c` file, you will understand if you are a C programmer). The second line imports the basic functionality in **Go** to talk with **C**.

Now make a method / function which will call the **C** function and return the result. This is for making the main method clean.

```go
func makeSquare(a int) int {
  //Convert Go ints to C ints
  aC := C.int(a)

  sum, err := C.square(aC)
  if err != nil {
    return 0
  }

  //Convert C.int result to Go int
  res := int(sum)

  return res
}
```

You can easily understand what this method does. It converts integer argument **a** for **Go** to integer argument for **C**. And also converts the returned result from **C** integer to **Go** integer.

So, we can call the function from the main function like:

```go
func main() {
  fmt.Print(makeSquare(9))
}
```

Now if we run `go run main.go` we will see the output `81` in the console.

That's the basic of calling a **C** function from **Golang**. For dealing with complex **C** programs which uses thirdparty libraries, your have to maintain **CFLAGS** and **LDFLAGS** from your go application. We will see that in another article.

You can find the sample source code here [https://github.com/mazhar266/goc](https://github.com/mazhar266/goc)