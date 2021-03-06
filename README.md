[Elasticsearch Overview](#Elasticsearch-Overview) 

[Creating an Inverted Index](#Creating-an-inverted-index)

[Elasticsearch Documents](#Elasticsearch-Documents)

[Indexing, Retrieving and Deleting Documents](#Indexing,-Retrieving-and-Deleting-Documents)

[Dev Console](#Dev-Console)

[Using GET Method](#Using-GET-Method)

[Using HEAD Method](#Using-HEAD-Method)

[Using PUT Method](#Using-PUT-Method)

[Using UPDATE API](#Using-UPDATE-API)

[Using DELETE Method](#Using-DELETE-Method)

[Components of an Index](#Components-of-an-Index)

[Term Query](#Term-Query)

[Match All Query](#Match-All-Query)

[Mapping Date Formats](#Mapping-Date-Formats)

[Retrieve Data from Index by Date Range](#Retrieve-Data-from-Index-by-Date-Range)

[Multi get mget API](#Multi-get-mget-API)

[Index Settings and Mapping](#Index-Settings-and-Mapping)

[Adding dynamic property to index](#Adding-dynamic-property-to-index)

[Understanding Analyzer](#Understanding-Analyzer)

[Search DSL Query Context](#Search-DSL-Query-Context)

[Query Context](#Query-Context)

[Search DSL Query Context Part 2](#Search-DSL-Query-Context-Part-2)

[Range Query](#Range-Query)

[Search DSL Filter Context](#Search-DSL-Filter-Context)

[Aggregations DSL Part 1](#Aggregations-DSL-Part-1)

[Aggregations DSL Part 2](#Aggregations-DSL-Part-2)

[Download and Configure Logstash](#Download-and-Configure-Logstash)

[Logstash Overview and Indexing Apache Application Logs](#Logstash-Overview-and-Indexing-Apache-Application-Logs)

# Elasticsearch Overview

![](./IMAGES/image1.png)


- The Primary aim of elasticsearch is to **search** for documents.
- Elasticsearch is like a search engine. they retrive documents at realtime.
- Searching is very fast in elasticsearch.
- Elasticsearch uses a data structure called an inverted index that supports very fast full-text searches. 
- An inverted index lists every unique word that appears in any document and identifies all of the documents each word occurs in.

### ElasticSearch Console
[Elasticsearch_Console_Link](http://localhost:5601/app/dev_tools#/console?load_from=https:/www.elastic.co/guide/en/elasticsearch/reference/current/snippets/1957.console
)

[Elasticsearch console](http://localhost:5601/app/dev_tools#/console)

# Create an Inverted Index

- To create an inverted index, we first split the content field of each document into separate words.
- Create a sorted list of all the unique terms.
- Then list the documents in which each term occurs.
- The result looks like this:

![](./IMAGES/image2.png)


# Elasticsearch Documents

Elasticsearch documents are stored in a JSON format.

![](./IMAGES/image3.png)

Here employees array contains 2 documents.

Documents also contain reserved fields that constitute the document metadata such as:

- **_index** ??? the index where the document resides
- **_type** ??? the type that the document represents
- **_id** ??? the unique identifier for the document


An Example of a document:

```json
{
   "_id": 3,
   "_type": [???your index type???],
   "_index": [???your index name???],
   "_source":{
   "age": 28,
   "name": ["daniel???],
   "year":1989,
  }
}
```

# Indexing, Retrieving and Deleting Documents

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

# Dev Console 

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

# Using GET Method

```bash
GET /vehicles/_doc/1
```

Retrive document with id 1.

```bash
GET /vehicles/_doc/2
```

Retrieve document with id 2.

Result will look like this:

```json
{
  "_index" : "vehicles",
  "_type" : "_doc",
  "_id" : "2",
  "_version" : 1,
  "_seq_no" : 6,
  "_primary_term" : 19,
  "found" : true,
  "_source" : {
    "make" : "yamaha",
    "milage" : 234,
    "color" : "black"
  }
}
```

here **_source** field contains the document.

To get only **_source** field as output, we can use the following query:

```bash
GET /vehicles/_doc/2/_source
```

Here we get the document without the meta fields.

# Using HEAD Method

To check the existence of document, we use HEAD method.

```bash
HEAD /vehicles/_doc/1
```

- If document exist, we will get a 200 status code.
- if documnet does not exist, we will get a 404 status code.

# Using PUT method

To update the document, we use the PUT method.

GET /vehicles/_doc/1

```bash
"_source" : {
    "make" : "honda",
    "milage" : 8722,
    "color" : "red"
  }
```

We have to update milage field of document with id 1.

```bash
PUT /vehicles/_doc/1
{
  "make": "honda",
  "milage": 6000,
  "color": "red"
}
```

- When we update using PUT method, it not only update that specific field, but the entire document.
- It is different from databases where we can update specific fields only.
- Here we are re indexing the document means inserting a new document with the udpated fields.

We get the following response after updating the document:

```json
{
  "_index" : "vehicles",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 7,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  }
```

- **_version** field gets updated each time we run the same query.
- **result** field tells us whether the document is updated or not.

This how updating documents in elasticsearch works.


# Using UPDATE API

```bash
POST vehicles/_doc/1/_update
{
  "doc":
  {
    "milage": 8900
  }
}
```

- Use **_update** API to update the document.
- Use **POST** method to update the document.
- Field that needed to be updated is specified in the **doc** field.
- We can update multiple fields at a time.
- Here also we are fully reindexing the document and not just updating the fields.

```bash
POST vehicles/_doc/2/_update
{
  "doc":{
    "make": "hero",
    "color": "red"
  }
}
```

Result will look like this:

```bash
{
  "_index" : "vehicles",
  "_type" : "_doc",
  "_id" : "2",
  "_version" : 2,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  }
```

- We can also insert new fields in the document.

```bash
POST vehicles/_doc/2/_update
{
  "doc":{
    "total_price": 780000
  }
}
```

- Inserted **total_price** field to the document with id 2. 

## Verify the changes in the document:


```bash
GET vehicles/_doc/2
```

- Result will look like this:

```bash
{
  "_index" : "vehicles",
  "_type" : "_doc",
  "_id" : "2",
  "_version" : 3,
  "_seq_no" : 11,
  "_primary_term" : 20,
  "found" : true,
  "_source" : {
    "make" : "hero",
    "milage" : 234,
    "color" : "red",
    "total_price" : 780000
  }
}
```

# Using DELETE Method

```bash
DELETE vehicles/_doc/2
```

- Here document with id 2 is deleted from index vehicles.

- Result will look like this:

```bash
{
  "_index" : "vehicles",
  "_type" : "_doc",
  "_id" : "2",
  "_version" : 4,
  "result" : "deleted",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  }
```

- Verify the document is deleted.

```bash
GET vehicles/_doc/2
```


- Result will look like this:

```bash
{
  "_index" : "vehicles",
  "_type" : "_doc",
  "_id" : "2",
  "found" : false
}
```

## Delete index using DELETE method

```BASH
DELETE vehicles/
```

Result will look like this:

```JSON
{
  "acknowledged" : true
}
```

# Components of an Index

## Create Index

Create index named *business* with type as *building*.

```bash
PUT /business/building/100
{
  "address": "56 New Dover",
  "floors": 10,
  "office": 21,
  "loc":{
    "lat": 60.8726378,
    "lon": 89.2198372
  }
}
```

### Get Structure of index

```bash
GET /business/
```

### Add another document to the index

```bash
PUT business/building/134
{
  "address": "89 New Dover",
  "floors": 11,
  "office": 90,
  "price": 7921.90,
  "loc":{
    "lat": 10.8726378,
    "lon": 49.2198372
  }
}
```

- This time we add another field **price** to the document.


### Add a document with different type to same index.

```bash
PUT /business/employee/344
{
  "name": "Jesse Ander",
  "title": "Accountant",
  "salary": 70000,
  "hiredate": "Jan 20, 2021"
}
```

- Shows exeception when we try to add another document with different type to same index.
- Since business index has type as building, we can not add another document with type as employee.
- We have to create new index with type as employee.
- An index can support only one type.

### Create Employee Index

```bash
PUT /employee/_doc/344
{
  "name": "Jesse Ander",
  "title": "Accountant",
  "salary": 70000,
  "hiredate": "Jan 20, 2021"
}
```

**Index:** employee

**Type:** _doc


## Search for Documents using _Search API

- We can search for business index and documents in it.

```bash
GET /business/_search/
```

Response will look like this:

```bash
  "hits" : {
    "total" : {
      "value" : 2,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "business",
        "_type" : "building",
        "_id" : "100",
        "_score" : 1.0,
        "_source" : {
          "address" : "56 New Dover",
          "floors" : 10,
          "office" : 21,
          "loc" : {
            "lat" : 60.8726378,
            "lon" : 89.2198372
          }
        }
      },
      {
        "_index" : "business",
        "_type" : "building",
        "_id" : "134",
        "_score" : 1.0,
        "_source" : {
          "address" : "89 New Dover",
          "floors" : 11,
          "office" : 90,
          "price" : 7921.9,
          "loc" : {
            "lat" : 10.8726378,
            "lon" : 49.2198372
          }
        }
      }
    ]
  }
}
```

- Here Documents are present in *hits.hits* array.

# Term Query

To search for a document matching a specific field.

```bash
GET /business/_search/
{
  "query": {
    "term": {
        "floors": 10
    }
  }
}
```

- Above query will search for documents with **floors** field equal to 10 and returns matching document.


**Example - 2**

```bash
GET /employee-api/_search/
{
  "query": {
    "term": {
      "role": "Developer"
    }
  }
}
```

- Above query will search for documents with **role** field equal to Developer and returns matching document.

# Match All Query

Returns every documents inside an index.

```bash
GET /employee-api/_search/
{
  "query": {
    "match_all": {}
  }
}
```

- **match-all** query returns every documents inside **employee-api** index.


**Example 2**

```bash
GET /employee/_search/
{
  "query": {
    "match_all": {}
  }
}
```


- **match-all** query returns every documents inside **employee** index.


# Mapping Date Formats

- Create an index where we give type to each fields. For date field, we input *type* and *format*.

```bash
  PUT purcharse-index
  {
    "mappings": {
      "properties": {
        "productId": {
          "type": "integer"
        },
        "name": {
          "type": "text"
        },
        "price": {
          "type": "float"
        },
        "purchaseDate": {
          "type": "date",
          "format": "yyyy-MM-dd"
        }
      }
    }
  }
```

### Get Data from Index

```bash
GET purcharse-index/_search
```

# Retrieve Data from Index by Date Range

- We pass date range in query. Set **lte** and **gte** property to set the start and end date.

```bash
GET /purcharse-index/_search
{
  "query": {
    "range": {
      "purchaseDate": {
        "gte": "2021-09-01",
        "lte": "2022-10-01"
      }
    }
  }
}
```

# Multi get (mget) API

- Retrieves multiple JSON documents by ID.
- Pass **index** and **id** property
```bash
GET /_mget
{
  "docs": [
    {
      "_index": "purcharse-index",
      "_id": 2
    },
    {
      "_index": "purcharse-index",
      "_id": 3
    }     
  ]
}
```

# Index Settings and Mapping

- Create new index with settings and mappings properties.

```bash
PUT customer
{
  "settings": {
    "number_of_shards": 2,
    "number_of_replicas": 1
  },
  "mappings": {}
}
```

- Define mapping for each field. We pass types and format of each field. We again use **PUT** method here. We specify **properties**. Inside **properties**, we specify the **fields**.

```bash
PUT /customer
{
  "mappings": {
    "properties": {
      "gender": {
        "type": "text",
        "analyzer": "standard"
      },
      "age": {
        "type": "integer"
      },
      "total_spent": {
        "type": "float"
      },
      "is_new": {
        "type": "boolean"
      },
      "name": {
        "type": "text",
        "analyzer": "standard"
      }
    }
  },
  "settings": {
    "number_of_shards": 2,
    "number_of_replicas": 1
  }
}
```

- *online* is the name of mapping.
- Inside *properties*, we specify the *fields*.
- *gender*, *age*, *total_spent*, *is_new* and *name* are the fields.

### Analyzer Property of a Field
**standard analyzer** splits the text on whitespace, lowercases it and removes non-alphanumeric characters.

Example:

```bash
"gender": {
  "type": "text",
  "analyzer": "standard"
}
```

### Get Document

```bash
GET customer/_search
```

### Insert a document to customer index.

```bash
PUT customer/_doc/1
{
  "gender": "male",
  "age": 22,
  "total_spent": 4000,
  "is_new": false,
  "name": "Imtiaz"
}
```

# Adding dynamic property to index.

Set dynamic property to mapping.

**dynamic**:

If set to **false** - Indexing field will be ignored. I.e. if we set **dynamic** to **false**, then we can't add new fields to index.

If set to **strict** - Indexing field will throw error. I.e. if we set **dynamic** to **strict**, then while adding a new field error is thrown to user.

To set dynamic propety to mapping we use PUT method. Pass dynamic property in **mappings**.

```bash
PUT customer
{
  "mappings": {
    "dynamic": "strict",
    "properties": {
        "online": {
          "properties": {
            "gender": {
              "type": "text",
              "analyzer": "standard"
            },
            "age": {
              "type": "integer"
            },
            "total_spent": {
              "type": "float"
            },
            "is_new": {
              "type": "boolean"
            },
            "name": {
              "type": "text",
              "analyzer": "standard"
            }
        }
      }
    }
  },
  "settings": {
    "number_of_shards": 2,
    "number_of_replicas": 1
  }
}
```

# Understanding Analyzer

## Whitespace Analyzer
```bash
POST _analyze
{
  "analyzer": "whitespace",
  "text": "The quick brown fox is here"
}
```

**Result :**

```bash
{
  "tokens" : [
    {
      "token" : "The",
      "start_offset" : 0,
      "end_offset" : 3,
      "type" : "word",
      "position" : 0
    },
    {
      "token" : "quick",
      "start_offset" : 4,
      "end_offset" : 9,
      "type" : "word",
      "position" : 1
    },
    {
      "token" : "brown",
      "start_offset" : 10,
      "end_offset" : 15,
      "type" : "word",
      "position" : 2
    },
    {
      "token" : "fox",
      "start_offset" : 16,
      "end_offset" : 19,
      "type" : "word",
      "position" : 3
    },
    {
      "token" : "is",
      "start_offset" : 20,
      "end_offset" : 22,
      "type" : "word",
      "position" : 4
    },
    {
      "token" : "here",
      "start_offset" : 23,
      "end_offset" : 27,
      "type" : "word",
      "position" : 5
    }
  ]
}
```

Here we can observe that text contents are broken up in to tokens based on the **whitespace** analyzer.


## Standard Analyzer

```bash
POST _analyze
{
  "analyzer": "standard",
  "text": "The quick brown fox is here"
}
```

**Result :**

```bash
{
  "tokens" : [
    {
      "token" : "the",
      "start_offset" : 0,
      "end_offset" : 3,
      "type" : "<ALPHANUM>",
      "position" : 0
    },
    {
      "token" : "quick",
      "start_offset" : 4,
      "end_offset" : 9,
      "type" : "<ALPHANUM>",
      "position" : 1
    },
    {
      "token" : "brown",
      "start_offset" : 10,
      "end_offset" : 15,
      "type" : "<ALPHANUM>",
      "position" : 2
    },
    {
      "token" : "fox",
      "start_offset" : 16,
      "end_offset" : 19,
      "type" : "<ALPHANUM>",
      "position" : 3
    },
    {
      "token" : "is",
      "start_offset" : 20,
      "end_offset" : 22,
      "type" : "<ALPHANUM>",
      "position" : 4
    },
    {
      "token" : "here",
      "start_offset" : 23,
      "end_offset" : 27,
      "type" : "<ALPHANUM>",
      "position" : 5
    }
  ]
}
```

Here we can observe that text contents are broken up in to tokens, also words are converted to lowercase by using **standard** analyzer.

## Simple Analyzer

Time: 9: 00

The **simple analyzer** breaks text into tokens at any non-letter character, such as numbers, spaces, hyphens and apostrophes, discards non-letter characters, and changes uppercase to lowercase.

```bash
POST _analyze
{
  "analyzer": "simple",
  "text": "The 2 QUICK Brown-Foxes jumped over the lazy dog's box."
}
```


**Result :**

```bash
{
  "tokens" : [
    {
      "token" : "the",
      "start_offset" : 0,
      "end_offset" : 3,
      "type" : "word",
      "position" : 0
    },
    {
      "token" : "quick",
      "start_offset" : 6,
      "end_offset" : 11,
      "type" : "word",
      "position" : 1
    },
    {
      "token" : "brown",
      "start_offset" : 12,
      "end_offset" : 17,
      "type" : "word",
      "position" : 2
    },
    {
      "token" : "foxes",
      "start_offset" : 18,
      "end_offset" : 23,
      "type" : "word",
      "position" : 3
    },
    {
      "token" : "jumped",
      "start_offset" : 24,
      "end_offset" : 30,
      "type" : "word",
      "position" : 4
    },
    {
      "token" : "over",
      "start_offset" : 31,
      "end_offset" : 35,
      "type" : "word",
      "position" : 5
    },
    {
      "token" : "the",
      "start_offset" : 36,
      "end_offset" : 39,
      "type" : "word",
      "position" : 6
    },
    {
      "token" : "lazy",
      "start_offset" : 40,
      "end_offset" : 44,
      "type" : "word",
      "position" : 7
    },
    {
      "token" : "dog",
      "start_offset" : 45,
      "end_offset" : 48,
      "type" : "word",
      "position" : 8
    },
    {
      "token" : "s",
      "start_offset" : 49,
      "end_offset" : 50,
      "type" : "word",
      "position" : 9
    },
    {
      "token" : "box",
      "start_offset" : 51,
      "end_offset" : 54,
      "type" : "word",
      "position" : 10
    }
  ]
}
```

# Search DSL Query Context

**DSL** - Domain Specific Language

Create *course* index with type *classroom*.
A sample index is given below:

```bash
PUT /courses/classroom/4
{
    "name": "Computer Science 101",
    "room": "C12",
    "professor": {
        "name": "Gregg Payne",
        "department": "engineering",
        "facutly_type": "full-time",
        "email": "payneg@onuni.com"
        },
    "students_enrolled": 33,
    "course_publish_date": "2013-08-27",
    "course_description": "CS 101 is a first year computer science introduction teaching fundamental data structures and alogirthms using python. "
}
```

## Search DSL Components

**Query Context:**

  This is used for full text searches. 

**Filter Context:**

  This is used for filtering the results.


# Query Context

## Match All Query

Returns all documents in the index.

```bash
GET courses/_search
{
  "query": {
    "match_all": {}
  }
}
```

## Match Query

Returns the documents that match the query. Here it returns the documents having name field as *computer*.

```bash
GET courses/_search
{
  "query": {
    "match": {
      "name": "computer"
    }
  }
}
```

Returns the documents with department of professor as *engineering*.

```bash 
GET courses/_search
{
  "query": {
    "match": {
      "professor.department": "engineering"
    }
  }
}
```

## Match Multiple Queries

Returns the documents that matches bpth match query. For example, if want to get documents which having fields name as *computer* and department as *engineering*.

We couple *must* array with *match* query in this case. Later we dump this *must* list into *bool* array.

**Console:**

```bash
GET courses/_search
{
  "query": {
    "bool": {
      "must": [
        {"match": {
          "name": "computer"
        }},
        {"match": {
          "room": "c8"
        }}
      ]
    }
  }
}
```

Here we return the documents from **courses** index which having fields name as *computer* and room as *c8*.

# Search DSL Query Context Part 2

## Must Not Condition

Set **must not** condition in query to return documents which doesn't match the query.

Below we get the documents which doesnt have department of professor as *engineering*.

```bash
GET courses/_search
{
  "query": {
    "bool": {
      "must_not": [
        {"match": {
          "professor.department": "engineering"
        }}
      ]
    }
  }
}
```

## Multi Match Query

We can use **multi-match** query to search for documents that matches the given query. Here we specify the **multiple fields** and query to search in.

```bash
GET courses/_search
{
  "query": {
    "multi_match": {
      "query": "finance",
      "fields": ["name", "professor.department"]
    }
  }
}
```

- Here we get documents with name as *finance* or professor department as *finance*. So either of these fields must match the query.

**Example - 2**

```bash
GET courses/_search
{
  "query": {
    "multi_match": {
      "query": "e3",
      "fields": ["room", "professor.email"]
    }
  }
}
```

- Here we get documents with room as *e3* or professor email as *e3*. So either of these fields must match the query.

## Match Phrase Query

Returns the documents that matches the phrase query. We specify the field and query to search in.

```bash
GET courses/_search
{
  "query": {
    "match_phrase": {
      "course_description": "crucial topics related to raising capital and bonds"
    }
  }
}
```
Here we search for documents where course_description field contains the phrase *crucial topics related to raising capital and bonds*.


**Example - 2**

```bash
GET courses/_search
{
  "query": {
    "match_phrase": {
      "course_description": "teaches students how to read"
    }
  }
}
```

Here we search for documents where course_description field contains the phrase *teaches students how to read*.

time: 14: 50

# Range Query

To return documents that matches the range query. We specify the field and range.
range is specified on student_enrolled field here. 

**gte** - greater than or equal to.

**lte** - less than or equal to.

```bash
GET courses/_search
{
  "query": {
    "range": {
      "students_enrolled": {
        "gte": 10,
        "lte": 20
      }
    }
  }
}
```

**Example - 2**

```bash
GET courses/_search
{
  "query": {
    "range": {
      "students_enrolled": {
        "gt": 20,
        "lt": 30
      }
    }
  }
}
```

## Range Query on Dates

- Here we specify the range on course_publish_date field. Return documents where course_publish_date is greater than or equal to 2015-01-01 and less than or equal to 2016-01-01.

```bash
GET courses/_search
{
  "query": {
    "range": {
      "course_publish_date": {
        "gte": "2015-01-01",
        "lte": "2016-01-01"
      }
    }
  }
}
```

# Search DSL Filter Context


## Using filter and match Query

Filter context is used to filter the results.

Here we filter out the documents that have name field as *accounting*.

```bash
GET courses/_search
{
  "query": {
    "bool": {
      "filter": [
        {"match": {
          "name": "accounting" 
          }
        }
      ]
    }
  }
}
```

**Example - 2**

Here we filter out the documents having department of professor as *finance*.

```bash
GET courses/_search
{
  "query": {
    "bool": {
      "filter": [
        {"match": {
          "professor.department": "finance" 
        }}
      ]
    }
  }
}
```

## Using multiple match queries with filter

**Task :**

Filter out the documents where **room** field is *e3* and **faculty type** of professor is *finance*.

```bash

```bash
GET courses/_search
{
  "query": {
    "bool": {
      "filter": [
        {"match": {
          "room": "e3"
        }},
        {
          "match": {
            "professor.facutly_type": "part-time"
          }
        }
      ]
    }
  }
}
```

## Using match and date range query with filter.

**Task:** 

Filter out the documents where **name** matches *accounting* and **course_publish_date** is greater than or equal to *2015-01-01* and less than or equal to *2016-01-01*.

```bash
GET courses/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "match": {
            "name": "accounting"
          }
        },
        {
          "range": {
            "course_publish_date": {
              "gte": "2015-01-01",
              "lte": "2016-01-01"
            }
          }
        }
      ]
    }
  }
}
```

## Using match and must not query with filter.

**Task:**

Filter out the documents where **name** matches *accounting* and **professor department** is not *finance*.

```bash
GET courses/_search
{
  "query": {
    "bool": {
      "filter": {
        "bool": {
          "must": [
            {
              "range": {
                "student_enrolled": {
                  "gte": 20
                }
              }
            }
          ]
        }
      },
      "should": [
        {
        "match": {
          "professor.department": 
            "finance"
        }
      }
    ]
    }
  }
}
```

# Aggregations DSL Part 1

## Bulk Insert of Documents

```bash
POST /vehicles/cars/_bulk
{ "index": {}}
{ "price" : 10000, "color" : "white", "make" : "honda", "sold" : "2016-10-28", "condition": "okay"}
{ "index": {}}
{ "price" : 20000, "color" : "white", "make" : "honda", "sold" : "2016-11-05", "condition": "new" }
{ "index": {}}
{ "price" : 30000, "color" : "green", "make" : "ford", "sold" : "2016-05-18", "condition": "new" }
{ "index": {}}
{ "price" : 15000, "color" : "blue", "make" : "toyota", "sold" : "2016-07-02", "condition": "good" }
{ "index": {}}
{ "price" : 12000, "color" : "green", "make" : "toyota", "sold" : "2016-08-19" , "condition": "good"}
{ "index": {}}
{ "price" : 18000, "color" : "red", "make" : "dodge", "sold" : "2016-11-05", "condition": "good"  }
{ "index": {}}
{ "price" : 80000, "color" : "red", "make" : "bmw", "sold" : "2016-01-01", "condition": "new"  }
{ "index": {}}
{ "price" : 25000, "color" : "blue", "make" : "ford", "sold" : "2016-08-22", "condition": "new"  }
{ "index": {}}
{ "price" : 10000, "color" : "gray", "make" : "dodge", "sold" : "2016-02-12", "condition": "okay" }
{ "index": {}}
{ "price" : 19000, "color" : "red", "make" : "dodge", "sold" : "2016-02-12", "condition": "good" }
{ "index": {}}
{ "price" : 20000, "color" : "red", "make" : "chevrolet", "sold" : "2016-08-15", "condition": "good" }
{ "index": {}}
{ "price" : 13000, "color" : "gray", "make" : "chevrolet", "sold" : "2016-11-20", "condition": "okay" }
{ "index": {}}
{ "price" : 12500, "color" : "gray", "make" : "dodge", "sold" : "2016-03-09", "condition": "okay" }
{ "index": {}}
{ "price" : 35000, "color" : "red", "make" : "dodge", "sold" : "2016-04-10", "condition": "new" }
{ "index": {}}
{ "price" : 28000, "color" : "blue", "make" : "chevrolet", "sold" : "2016-08-15", "condition": "new" }
{ "index": {}}
{ "price" : 30000, "color" : "gray", "make" : "bmw", "sold" : "2016-11-20", "condition": "good" }
```

## Get document - Using size property with query

```bash
GET vehicles/cars/_search
{
  "size": 20,
  "query": {
    "match_all": {}
  }
}
```

## Using Sort in Query 

Sort by price in descending order.

```bash
GET vehicles/cars/_search
{
  "from": 0,
  "size": 5,
  
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "price": {
        "order": "desc"
      }
    }
      
  ]
}
```

**Example - 2**

Sort by sold date in descending order.

```bash
GET vehicles/cars/_search
{
  "from": 0,
  "size": 5,
  
  "query": {
    "match_all": {}
  },
  
  "sort": [
    {
      "sold": {
        "order": "desc"
      }
    }
      
  ]
}
```

## Using Count API

Give the number of documents in the collection that match the query.

Here we get the count of documents with **make** field as *dodge*.

```bash
GET vehicles/cars/_count
{
  "query": {
    "match": {
      "make": "dodge"
    }
  }
}
```

## Using Aggregations

https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html


**Syntax:**

```bash
GET /my-index-000001/_search
{
  "size": 0,
  "aggs": {
    "my-agg-name": {
      "terms": {
        "field": "my-field"
      }
    }
  }
}
```

Here we can find that more cars are sold for makers *dodge*.

```bash
GET vehicles/cars/_search
{
  "aggs": {
    "popular_cars": {
      "terms": {
        "field": "make.keyword"
      }
    }
  }
}
```

**Result :**

```bash
  "aggregations" : {
    "popular_cars" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 0,
      "buckets" : [
        {
          "key" : "dodge",
          "doc_count" : 5
        },
        {
          "key" : "chevrolet",
          "doc_count" : 3
        },
        {
          "key" : "bmw",
          "doc_count" : 2
        },
        {
          "key" : "ford",
          "doc_count" : 2
        },
        {
          "key" : "honda",
          "doc_count" : 2
        },
        {
          "key" : "toyota",
          "doc_count" : 2
        }
      ]
    }
  }
```

## Using multiple fields in aggregations

Here we aggregate based on popular cars and average car prices.

```bash
GET /vehicles/cars/_search
{
  "aggs": {
    "popular_cars": {
      "terms": {
        "field": "make.keyword"
      }
    },
    "average_car_price": {
      "avg": {
        "field": "price"
      }
    }
  }
}
```

**Result :**

```bash
 "aggregations" : {
    "popular_cars" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 0,
      "buckets" : [
        {
          "key" : "dodge",
          "doc_count" : 5
        },
        {
          "key" : "chevrolet",
          "doc_count" : 3
        },
        {
          "key" : "bmw",
          "doc_count" : 2
        },
        {
          "key" : "ford",
          "doc_count" : 2
        },
        {
          "key" : "honda",
          "doc_count" : 2
        },
        {
          "key" : "toyota",
          "doc_count" : 2
        }
      ]
    },
    "average_car_price" : {
      "value" : 23593.75
    }
```

**Example - 2**

Aggregate on popular_cars, average_car_price and maximum price.

```bash
GET /vehicles/cars/_search
{
  "aggs": {
    "popular_cars": {
      "terms": {
        "field": "make.keyword"
      }
    },
    "average_car_price": {
      "avg": {
        "field": "price"
      }
    },
    "max_price": {
      "max": {
        "field": "price"
      }
    }
  }
}
```

## Using query context with aggregations

Here we apply the aggregations on documents that have field *color* as red.

```bash
GET /vehicles/cars/_search
{
  "query": {
    "match": {
      "color": "white"
    }
  },
  "aggs": {
    "popular_cars": {
      "terms": {
        "field": "make.keyword"
      }
    },
    "average_car_price": {
      "avg": {
        "field": "price"
      }
    },
    "min_price": {
      "min": {
        "field": "price"
      }
    },
    "max_price": {
      "max": {
        "field": "price"
      }
    }
  }
}
```

# Aggregation DSL Part 2

## Stats Property

**stats** property helps to perform all the aggregations at once.

```bash
GET vehicles/cars/_search
{
  "aggs": {
    "popular_cars": {
      "terms": {
        "field": "make.keyword"
      },
    "aggs": {
      "stats_on_price": {
        "stats": {
          "field": "price"
          }
        }
      }
    }
  }
}
```

**Result Look like this**

```bash
{
  "key" : "dodge",
  "doc_count" : 5,
  "stats_on_price" : {
    "count" : 5,
    "min" : 10000.0,
    "max" : 35000.0,
    "avg" : 18900.0,
    "sum" : 94500.0
  }
}
```

Count, min, max, avg, sum are the properties of the stats object. So we get all aggregations at once.

*stats_on_price* - default name given to aggregation.


## Using range query with aggregations

Return the documents based on fields date range and sold.

```bash
GET vehicles/cars/_search
{
  "aggs": {
    "popular_cars": {
      "terms": {
        "field": "make.keyword"
      },
      "aggs": {
        "sold_date_ranges": {
          "range": {
            "field": "sold",
            "ranges": [
              {
                "from": "2016-01-01",
                "to": "2016-05-18"
              },
              {
                "from": "2016-05-18",
                "to": "2017-01-01"
              }
            ]
          }
        }
      }
    }
  }
}
```

**Task:** 

Find average price of vehicles sold in the given date range.


```bash
GET vehicles/cars/_search
{
  "aggs": {
    "popular_cars": {
      "terms": {
        "field": "make.keyword"
      },
      "aggs": {
        "sold_date_ranges": {
          "range": {
            "field": "sold",
            "ranges": [
              {
                "from": "2016-01-01",
                "to": "2016-05-18"
              },
              {
                "from": "2016-05-18",
                "to": "2017-01-01"
              }
            ]
          }
        },
        "average_car_price": {
          "avg": {
            "field": "price"
          }
        }
      }
    }
  }
}
```

# Download and Configure Logstash

![](./IMAGES/image_22.png)

**Description :**

First stage is to get the data into the pipeline from the *Data Sources*. Then data goes into *Inputs*. After, there is a filtering process where we can filter out the data we want and ignore the data we dont want. Through *Outputs* data reaches the *Data Destinations*. So there are 3 stages:

1. **Inputs**
2. **Filters**
3. **Outputs**
  
## Install and Configure Logstash

1. Go to below link:

https://www.elastic.co/downloads/logstash

2. Choose platform and download the file.

3. Extract the file.

4. Open the terminal in which folder you extracted the file.

5. Navigate to the logstash folder.

6. Run the following command:
 
```bash
$ bin/logstash -e 'input { stdin {}} output {stdout {}}'
```

# Logstash Overview and Indexing Apache Application Logs




Time - 7:00

Lecture: https://www.udemy.com/course/complete-elasticsearch-masterclass-with-kibana-and-logstash/learn/lecture/7251304#overview







