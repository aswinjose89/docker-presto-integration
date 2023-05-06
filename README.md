# docker-presto-integration
PrestoDB with external database integration.

# Data lakehouse
Used to maintain Structured, Semi-structured  and unstructured data permanently


# Prerequisites
 Tested in Docker Version 20.x.x, Docker Compose version 1.29.x 

## ✨ Mongo with Prestodb Integration [Watch on YouTube]

This repository was created to accomodate my YouTube tutorial:
https://youtu.be/42pjHGjrpPs

[![Mongo with Prestodb Integration](https://img.youtube.com/vi/42pjHGjrpPs/0.jpg)](https://youtu.be/42pjHGjrpPs "Mongo with Prestodb Integration")


# How to run docker compose

## Run Presto with mongodb
```
    docker-compose -f docker-compose-presto-mongo.yml up
``` 

## Run Presto with mongodb and mysql
```
    docker-compose -f docker-compose-presto-mysql-mongo.yml up
``` 

|Description                         |Docker Execution Command |
|-------------------------------|-----------------------------|
|To Connect Prestodb            |docker exec -it presto presto-cli |
|To connect mongoDB            |docker exec -it presto-mongo mongosh |
|To connect mysql|docker exec -it presto-mysql mysql -h localhost -u root -p|

Sample MongoDB Query from MongoDB
```
use presto_to_mongodb;
db.createCollection("book")
db.book.insertOne({"id":1, "book_name":"harry potter" })
db.book.insertOne({"id":2, "book_name":"the wicked king" })
```

Sample SQL Query from mysql
```
CREATE DATABASE presto_to_mysql;
show databases;
use presto_to_mysql;
show tables;
create table author(id bigint, author varchar(255));
insert into author values(1, "Rowlings");
insert into author values(2, "Holly Black");
```

Sample query to query mongodb and mysql from PrestoDB
```
show catalogs;
-----------------------------------
show schemas in mongodb;
use mongodb.presto_to_mongodb;
select * from book;
-----------------------------------
show schemas in mysql;
use mysql.presto_to_mysql;
select * from author;
```

Join Query between mongodb and mysql from prestodb;
```
select * from mysql.presto_to_mysql.author A join mongodb.presto_to_mongodb.book B on A.id=B.id;
```