# PrestoDB with External DB
PrestoDB with external database integration using docker compose file. PrestoDB is used to maintain Structured, Semi-structured  and unstructured data permanently

# Prerequisites
 Tested in Docker Version 20.x.x, Docker Compose version 1.29.x 

# Entire Docker Compose Commands

## ✨ Run Presto with mongodb

Watch youtube video for more details
https://youtu.be/42pjHGjrpPs

[![Mongo with Prestodb Integration](https://img.youtube.com/vi/42pjHGjrpPs/0.jpg)](https://youtu.be/42pjHGjrpPs "Mongo with Prestodb Integration")

Docker Compose command
```
    docker-compose -f docker-compose-presto-mongo.yml up
``` 

## ✨ Run Presto with mongodb and mysql

- Connecting Prestodb, mongodb & mysql
- Creating individual tables in mongodb and prestidb
- Query data from prestodb to retrieve data from mongodb and mysql
- Create a join query from prestodb for the table between mongodb & mysql

Watch youtube video for more details
https://youtu.be/lFg3rtQq-ug

[![Mongo and Mysql with Prestodb Integration](https://img.youtube.com/vi/lFg3rtQq-ug/0.jpg)](https://youtu.be/lFg3rtQq-ug "Mongo with Prestodb Integration")

Docker Compose command
```
    docker-compose -f docker-compose-presto-mysql-mongo.yml up
``` 
After docker compose command follow the below commands to execute the container

|Description                         |Docker Execution Command |
|-------------------------------|-----------------------------|
|To Connect Prestodb            |docker exec -it presto presto-cli |
|To connect mongoDB            |docker exec -it presto-mongo mongosh |
|To connect mysql|docker exec -it presto-mysql mysql -h localhost -u root -p|

Sample MongoDB Query from MongoDB Container
```
use presto_to_mongodb;
db.createCollection("book")
db.book.insertOne({"id":1, "book_name":"harry potter" })
db.book.insertOne({"id":2, "book_name":"the wicked king" })
```

Sample SQL Query from mysql Container
```
CREATE DATABASE presto_to_mysql;
show databases;
use presto_to_mysql;
show tables;
create table author(id bigint, author varchar(255));
insert into author values(1, "Rowlings");
insert into author values(2, "Holly Black");
```

Sample query to manipulate mongodb and mysql from PrestoDB
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

## ✨ Run Presto with kafka

Docker Compose command
```
    docker-compose -f docker-compose-presto-kafka.yml up
``` 

After docker compose command follow the below commands to execute the container

|Description                         |Docker Execution Command |
|-------------------------------|-----------------------------|
|To Connect Prestodb            |docker exec -it presto presto-cli |
|To Connect Kafka            |docker exec -it presto-kafka bash |

Commands for showing list of topics, Consume selected topic and Produce messages in selected topic
```
docker exec -it presto-kafka bash /bin/kafka-topics --list --bootstrap-server localhost:9092

docker exec -it presto-kafka bash /bin/kafka-console-consumer --topic codespotify.presto_kafka_topic --bootstrap-server localhost:9092

docker exec -it presto-kafka bash /bin/kafka-console-producer --topic codespotify.presto_kafka_topic --bootstrap-server localhost:9092
```

Query to execute from PrestoDB to manipulate kafka messages
```
show schemas in kafka.codespotify;
select _message from kafka.codespotify.presto_kafka_topic;
```

## ✨ Run Presto with Elasticsearch

Docker Compose command
```
    docker-compose -f docker-compose-presto-elasticsearch.yml up
``` 

After docker compose command follow the below commands to execute the container

|Description                         |Docker Execution Command |
|-------------------------------|-----------------------------|
|To Connect Prestodb            |docker exec -it presto presto-cli |

Create new Elasticsearch Index
```
curl -X PUT -H "Content-Type: application/json" http://localhost:9200/presto_es_index/ | json_pp
```

Search records in Elasticsearch Index
```
curl -X GET -H "Content-Type: application/json" http://localhost:9200/presto_es_index/_search | json_pp
```

Create new record in Elasticsearch
```
curl -X POST -H "Content-Type: application/json" http://localhost:9200/presto_es_index/_doc -d '{ "youtube" : "Welcome To CodeSpotify", "myname": "Aswin"}' | json_pp
```

Query to execute from PrestoDB to manipulate Elasticsearch index
```
show catalogs;
show schemas in elasticsearch;
select * from elasticsearch.presto_es_index.presto_es_index;
```