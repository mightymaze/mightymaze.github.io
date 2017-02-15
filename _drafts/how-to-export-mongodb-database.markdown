---
layout: post
title: How to export MongoDB Database
tags:
- mongo
- mongodb
---

```
$ cat > locations.csv
Name,Address,City,State,ZIP
Jane Doe,123 Main St,Whereverville,CA,90210
John Doe,555 Broadway Ave,New York,NY,10010
 ctrl-d
$ mongoimport -d mydb -c things --type csv --file locations.csv --headerline
connected to: 127.0.0.1
imported 3 objects
$ mongo
MongoDB shell version: 1.7.3
connecting to: test
> use mydb
switched to db mydb
> db.things.find()
{ "_id" : ObjectId("4d32a36ed63d057130c08fca"), "Name" : "Jane Doe", "Address" : "123 Main St", "City" : "Whereverville", "State" : "CA", "ZIP" : 90210 }
{ "_id" : ObjectId("4d32a36ed63d057130c08fcb"), "Name" : "John Doe", "Address" : "555 Broadway Ave", "City" : "New York", "State" : "NY", "ZIP" : 10010 }
```