# Elasticsearch Oevrview

![](./IMAGES/image1.png)


- The Primary aim of elasticsearch is to **search** for documents.
- Elasticsearch is like a search engine. they retrive documents at realtime.
- Searching is very fast in elasticsearch.
- Elasticsearch uses a data structure called an inverted index that supports very fast full-text searches. 
- An inverted index lists every unique word that appears in any document and identifies all of the documents each word occurs in.

## Elastoc search Console
[Elasticsearch_Console_Link](http://localhost:5601/app/dev_tools#/console?load_from=https:/www.elastic.co/guide/en/elasticsearch/reference/current/snippets/1957.console
)

[Elasticsearch console](http://localhost:5601/app/dev_tools#/console)

## Creating an inverted index

- To create an inverted index, we first split the content field of each document into separate words.
- Create a sorted list of all the unique terms.
- Then list the documents in which each term occurs.
- The result looks like this:

![](./IMAGES/image2.png)


## Elasticsearch Documents

Elasticsearch documents are stored in a JSON format.

![](./IMAGES/image3.png)

Here employees array contains 2 documents.

Documents also contain reserved fields that constitute the document metadata such as:

- **_index** – the index where the document resides
- **_type** – the type that the document represents
- **_id** – the unique identifier for the document


An Example of a document:

```json
{
   "_id": 3,
   "_type": [“your index type”],
   "_index": [“your index name”],
   "_source":{
   "age": 28,
   "name": ["daniel”],
   "year":1989,
}
}

```

## Indexing, Retriving and Deleting Documents

- In Elasticsearch, data is stored in to **index**.

- Consider an example of vehicle index. In this index, we can store vehicle documents.

- We store documents to **index**.

- Each **key:value** pair in documents is a **field**.


![](./IMAGES/image4.png)

1. First, Here we created a vehicle index.
2. Stored multiple Car documents to vehicle index.

- In Elasticsearch, inserting a data is called **indexing**.

 
## Indexing Document

Syntax for indexing a document.

![](./IMAGES/image5.png)

**Terminologies**

- **PUT** is the HTTP method used to index a document.
- **index** is the name of the index.
- **type** is the type of the document.
- **id** is the unique identifier for the document.
- With in curly braces, we provide the fields.

Time: 6: 30

lecture: https://www.udemy.com/course/complete-elasticsearch-masterclass-with-kibana-and-logstash/learn/lecture/7283502#overview

## Dev Console 

```console
PUT /vehicles/_doc/1
{
  "make": "honda",
  "milage": 8722,
  "color": "red"
}
```

vehicles - index name,

_doc - document type,

1 - document id

- json part is the document we gave to elasticsearch.

- Fields starting underscore are **meta fields**.
eg: _idnex, _type, _id, _shards 

- **_version** field gets updated each time we run the same query.

**Success Message after creating index**:

```json
{
  "_index" : "vehicles",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 3,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 2,
  "_primary_term" : 1
}
```

Time: 13:00
