---
title: Generate REST API from DB Schema in Crystal
tags:
- crystal
- haskell
---

I had a MySQL schema from a booking.com like platform. I wanted to generate REST API very fast from my MySQL database tables. Like I have a table named `hotels`. I want to generate following endpoints:

```
GET    /api/hotels
GET    /api/hotels/:id
POST   /api/hotels
PUT    /api/hotels/:id
DELETE /api/hotels/:id
```


I tried to use **Haskell** but the security was very low on `postgrest` package in **Haskell**.  The **postgrest** is amazing, it generates the api endpoints from a database. But the problem is it will delete all the rows in a table if I perform a *DELETE* request in `/api/hotels`. So, I can not use it.

Finally found that `Kemal Rest API` is the perfect for doing this which is on **Crystal Lang**. Suppose I have a table named `hotels`. This is what I did. I installed **Crystal Lang**. I ran `shards init app hotel` to generate an empty project. `shards` is the dependency manager, task runner for crystal. So after this I have a folder named hotel which has the following structure:
```
.
├── LICENSE
├── README.md
├── db.sql
├── shard.lock
├── shard.yml
├── spec
│   ├── hotel_spec.cr
│   └── spec_helper.cr
└── src
    └── hotel.cr
```

I have modified the `shard.yml` file and added following lines at the end.

```
dependencies:
  db:
    github: crystal-lang/crystal-db
  kemal:
    github: kemalcr/kemal
  kemal-rest-api:
    github: blocknotes/kemal-rest-api
  mysql:
    github: crystal-lang/crystal-mysql
```

I have ran the `shards install` to install the libraries and modified the `hotel.cr` file like following:

```crystal
require "db"
require "mysql"
require "kemal"
require "kemal-rest-api"
require "json"

DB_NAME       = "hotel"
DB_CONNECTION = "mysql://hotel@localhost/#{DB_NAME}"

class MyModel < KemalRestApi::Adapters::CrystalDbModel
  def initialize
    super DB_CONNECTION, "hotels"
  end
end

module WebApp
  res = KemalRestApi::Resource.new MyModel.new, KemalRestApi::ALL_ACTIONS, prefix: "api", singular: "hotel"

  # Routes
  get "/" do |env|
    env.response.content_type = "application/json"
    {name: "--", info: "--", version: "0.0.1"}.to_json
  end

  res.generate_routes!

  # Starts Kemal
  Kemal.run
end

```

to generate these routes automatically:
```
GET    /api/hotels
GET    /api/hotels/:id
POST   /api/hotels
PUT    /api/hotels/:id
DELETE /api/hotels/:id
```

At last I have ran `shards build` to make the binary executable file. And ran it like `./bin/hotel`. And the API is working properly.

The **Crystal** code is just like **Ruby** language but as fast as **C**. What I need to do next is make an array of tables and it will generate the API endpoints. I don't need to take hassle about images and users because we already have other microservices to handle those. Also, this API will never be exposed to clients rather another API wrapper will fetch data from it. So, I found it perfect fit for my job.

Thus I have made my hotel API up in just couple of hours.