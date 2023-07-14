# elk

## 18. Creating & deleting indices

To create an index named "my_index":



```
PUT /my_index

```

To delete an index named "my_index":



```
DELETE /my_index

```

## 19. Indexing documents

To index a document with ID "1" in the index "my_index":



```
POST /my_index/_doc/1
{
  "title": "Elasticsearch Course",
  "description": "Learn how to use Elasticsearch and Kibana"
}

```

The response will contain information about the  indexing operation, including the  document ID  and  index name.

Elasticsearch  will automatically create an index "my_index" if it doesn't exist.

## 20. Retrieving documents by ID

To retrieve the document with  ID  "1" from the index "my_index":



```
GET /my_index/_doc/1

```

The response will contain the document content in  JSON format.

## 21. Updating documents

To update the document with ID "1" in the index "my_index":

```
PUT /my_index/_doc/1
{
  "title": "Updated Elasticsearch Course",
  "description": "Learn how to use Elasticsearch and Kibana for search and analytics"
}

```

The response will contain information about the update operation, including the document ID and index name.

## 22. Scripted updates

To update the "description" field of the document with ID "1" in the index "my_index" using a script:

```
POST /my_index/_update/1
{
  "script": {
    "source": "ctx._source.description = params.new_description",
    "params": {
      "new_description": "Learn how to use Elasticsearch and Kibana for search, analytics, and visualization"
    }
  }
}

```

In this example, Elasticsearch will only update the document if the  current version  number is "1". If the version number has been incremented by another update operation, Elasticsearch will return a  version conflict error.


The response will contain information about the update operation, including the document ID and index name. The script updates the "description" field with the new value provided in the "params" section.

# Elastic Search  and Kibana Course Notes

## 23. Upserts

-   An  upsert operation  updates a document if it exists, or creates a new document if it doesn't exist.
-   To perform an upsert operation, use the "upsert" parameter in the update request.

Example:


```
POST /my_index/_update/1
{
  "doc": {
    "title": "Updated Elasticsearch Course",
    "description": "Learn how to use Elasticsearch and Kibana for search and analytics"
  },
  "upsert": {
    "title": "New Elasticsearch Course",
    "description": "This is a new Elasticsearch course"
  }
}

```

In this example, Elasticsearch will try to update the document with ID "1" with the new title and description values. If the document doesn't exist, Elasticsearch will create a new document with the upsert values.

## 24. Replacing documents

-   A replace operation replaces a document with a new document.
-   To perform a replace operation, use the  index command  with the  document ID  and provide the new document in the request body.


```
PUT /my_index/_doc/1
{
  "title": "New Elasticsearch Course",
  "description": "This is a new Elasticsearch course"
}

```

In this example, Elasticsearch will replace the document with ID "1" with the new document provided in the request body.

## 25. Deleting documents

-   To delete a document, use the delete command with the  index name  and document ID.

Example:


```
DELETE /my_index/_doc/1

```

In this example, Elasticsearch will delete the document with ID "1" from the index "my_index".

## 26. Understanding routing

-   Routing is a way to group related documents together in the same shard.
-   Elasticsearch uses the document ID by default to determine which shard a document should be stored in.
-   By specifying a  routing value, you can control which shard a document should be stored in.

Example:


```
PUT /my_index/_doc/1?routing=user123
{
  "title": "Elasticsearch Course",
  "description": "Learn how to use Elasticsearch and Kibana",
  "user_id": "user123"
}

```

In this example, the document is stored in the shard that corresponds to the routing value "user123". This can be useful for improving  search performance  when querying documents by a specific user.

## 27. How Elasticsearch reads data

-   Elasticsearch uses a distributed architecture to store and retrieve data.
-   When a  search query  is executed, Elasticsearch sends the query to all the nodes in the cluster.
-   Each node returns a subset of the results, which are then combined and returned to the client.

## 28. How Elasticsearch writes data

-   When a document is indexed, Elasticsearch determines which shard it should be stored in based on the document ID and routing value.
-   The data is then written to the  primary shard  and replicated to the  replica shards.
-   Once the data has been successfully written to all the shards, the  indexing operation  is considered successful.

## 29. Understanding document versioning

-   Elasticsearch uses a version number to keep track of changes to a document.
-   The  version number  is incremented each time a document is updated.
-   The version number can be used to implement  optimistic concurrency control.

Example:


```
PUT /my_index/_doc/1
{
  "title": "Elasticsearch Course",
  "description": "Learn how to use Elasticsearch and Kibana"
}

```

In this example, Elasticsearch will assign the document version number "1" to the newly  indexed document.

## 31. Update by query

-   Update by query is a way to update multiple documents that match a query.
-   To perform an update by query, use the update_by_query API.

Example:


```
POST /my_index/_update_by_query
{
  "script": {
    "source": "ctx._source.title = params.new_title",
    "params": {
      "new_title": "Updated Elasticsearch Course"
    }
  },
  "query": {
    "match": {
      "description": "Elasticsearch"
    }
  }
}

```

In this example,  Elasticsearch  will update the "title" field of all documents that contain the word "Elasticsearch" in the "description" field.

## 32. Delete by query

-   Delete by query is a way to delete multiple documents that match a query.
-   To perform a delete by query, use the delete_by_query  API.

Example:


```
POST /my_index/_delete_by_query
{
  "query": {
    "match": {
      "description": "Elasticsearch"
    }
  }
}

```

In this example, Elasticsearch will delete all documents that contain the word "Elasticsearch" in the "description" field.

## 33. Batch processing

-   Batch processing  is a way to perform  multiple indexing, update, or delete operations in a single request.
-   Batch processing can improve  indexing performance  by reducing network overhead.
-   To perform batch processing, use the bulk API.

Example:


```
POST /my_index/_bulk
{ "index": { "_id": "1" } }
{ "title": "Elasticsearch Course", "description": "Learn how to use Elasticsearch and Kibana" }
{ "delete": { "_id": "2" } }

```

In this example, Elasticsearch will perform three operations in a single request: index a document with ID "1", delete the document with ID "2", and index a new document with an automatically generated ID.

## 34. Importing data with cURL

-   cURL is a command-line tool for transferring data using various protocols, including HTTP.
-   You can use cURL to import data into Elasticsearch from a file.
-   The  data file  should contain one  JSON object  per line.

Example:

```
curl -H "Content-Type: application/x-ndjson" -XPOST "localhost:9200/my_index/_bulk" --data-binary "@my_data.json"

```

In this example, cURL sends a bulk request to Elasticsearch to index all the documents in the "my_data.json" file. The file should contain one JSON object per line, as required by the  bulk API.



### Introduction to Analysis

-   Analysis is the process of converting text into terms that can be searched efficiently.
-   It involves tokenization, stemming, stopword removal, and other techniques that help to normalize the text.
-   Elasticsearch provides a powerful analysis engine that can be customized to meet the needs of different use cases.

### Using the Analyze API

-   The  Analyze API  allows you to test the analysis process for a specific text string.
-   You can use it to see how Elasticsearch will tokenize and normalize a piece of text, and to debug any issues with your  analysis configuration.
-   The syntax for the Analyze API is as follows:

```
GET /_analyze
{
  "analyzer": "standard",
  "text": "This is a sample text"
}

```

-   This will return the tokens produced by the standard analyzer for the given text:

```
{
  "tokens": [
    {
      "token": "this",
      "start_offset": 0,
      "end_offset": 4,
      "type": "<ALPHANUM>",
      "position": 0
    },
    {
      "token": "is",
      "start_offset": 5,
      "end_offset": 7,
      "type": "<ALPHANUM>",
      "position": 1
    },
    {
      "token": "a",
      "start_offset": 8,
      "end_offset": 9,
      "type": "<ALPHANUM>",
      "position": 2
    },
    {
      "token": "sample",
      "start_offset": 10,
      "end_offset": 16,
      "type": "<ALPHANUM>",
      "position": 3
    },
    {
      "token": "text",
      "start_offset": 17,
      "end_offset": 21,
      "type": "<ALPHANUM>",
      "position": 4
    }
  ]
}

```

### Understanding Inverted Indices

-   An  inverted index  is a  data structure  that allows for quick full-text searches.
-   It is used by Elasticsearch to store the terms in a document and the documents that contain each term.
-   Elasticsearch uses a  reverse index  to optimize search speed by storing the terms and their corresponding documents in memory.

### Introduction to Mapping

-   A mapping defines how documents and their fields are indexed and stored in Elasticsearch.
-   It specifies the data type of each field, how it should be analyzed, and other configuration options.
-   Elasticsearch automatically creates a mapping for each index based on the first document that is indexed.

### Overview of Data Types

-   Elasticsearch supports a variety of data types, including:
    
    -   Text: Used for full-text search and analysis.
    -   Keyword: Used for  exact matches  and aggregations.
    -   Long, Integer, Short, Byte, Double, Float, Half Float, Scaled Float: Used for numeric values.
    -   Date: Used for date and time values.
    -   Boolean: Used for true/false values.
    -   Binary: Used for storing binary data.
    -   Range: Used for ranges of numeric or date values.
    -   Object: Used for  nested objects.
    -   Nested: Used for arrays of objects.
    -   Geo-point: Used for latitude/longitude coordinates.
-   When defining a mapping, you must specify the data type for each field. For example:
    

```
PUT /my_index
{
  "mappings": {
    "properties": {
      "name": { "type": "text" },
      "price": { "type": "float" },
      "color": { "type": "keyword" },
      "created_at": { "type": "date" }
    }
  }
}

```

This creates an index called  `my_index`  with a mapping that defines four fields:  `name`,  `price`,  `color`, and  `created_at`. The  `type`  parameter specifies the data type for each field.

### How the "Keyword" Data Type Works

-   The  `keyword`  data type is used for fields that contain exact values.
-   By default, Elasticsearch treats text fields as full-text fields, which means they are analyzed and broken down into individual terms for searching.
-   In contrast,  `keyword`  fields are not analyzed and store the exact value of the field.
-   This makes them ideal for fields that are used for filtering, sorting, and aggregations.

### Understanding Type Coercion

-   Elasticsearch tries to convert values of one data type to another data type, based on the context in which they are used.
-   This is known as  type coercion.
-   For example, if you try to index a string value in a  numeric field, Elasticsearch will try to convert the string to a number before indexing it.
-   Type coercion can sometimes lead to unexpected results, so it's important to be aware of how it works.

### Understanding Arrays

-   Elasticsearch supports arrays, which allow you to store multiple values in a single field.
-   Arrays can be used for fields that have multiple values, such as tags, categories, or authors.
-   When querying an  array field, Elasticsearch returns any documents that contain at least one matching value in the array.
-   You can also use aggregations to group and analyze the values in an array.

### Adding Explicit Mappings

-   Elasticsearch automatically creates a mapping for each index based on the first document that is indexed.
-   However, it's often useful to define an  explicit mapping  that specifies the data type and analysis settings for each field.
-   You can define an explicit mapping when creating an index, or you can update the mapping for an existing index.
-   Explicit mappings are useful for ensuring  consistent data types  across documents, optimizing  search performance, and preventing unexpected type coercion.

Here's an example of how to add an explicit mapping to an index:

```
PUT /my_index/_mapping
{
  "properties": {
    "name": {
      "type": "text",
      "analyzer": "english"
    },
    "tags": {
      "type": "keyword"
    },
    "created_at": {
      "type": "date",
      "format": "yyyy-MM-dd HH:mm:ss"
    }
  }
}

```

This adds an explicit mapping for the  `my_index`  index, which defines three fields:  `name`,  `tags`, and  `created_at`. The  `type`  parameter specifies the data type for each field, and additional parameters can be used to specify  analysis settings,  date formats, and other configuration options.


## 46. Retrieving mappings

Mapping in Elasticsearch is the process of defining the schema or structure of your data. The mapping describes the fields in your documents, their data types, and how they should be indexed and stored. You can retrieve the mapping for an index using the  `_mapping`  API.

Here is an example of how to retrieve the mapping for an index named  `my_index`  using the  Elasticsearch Java API:

```
GetMappingsRequest request = new GetMappingsRequest();
request.indices("my_index");

GetMappingsResponse response = client.indices().getMapping(request, RequestOptions.DEFAULT);
Map<String, MappingMetaData> mappings = response.mappings();

```

This code sends a  `GET`  request to the  `_mapping`  API with the name of the index. The response contains a map of index names to their corresponding mappings.

## 47. Using dot notation in  field names

In Elasticsearch, you can use  dot notation  in field names to define nested fields. For example, if you have a document with a nested field called  `address`  with sub-fields  `street`,  `city`, and  `state`, you can define the field as  `address.street`,  `address.city`, and  `address.state`.

Here is an example of how to define  nested fields  using dot notation in a mapping:

```
{
  "mappings": {
    "properties": {
      "address": {
        "properties": {
          "street": { "type": "text" },
          "city": { "type": "text" },
          "state": { "type": "text" }
        }
      }
    }
  }
}

```

This mapping defines a  nested field  called  `address`  with sub-fields  `street`,  `city`, and  `state`.

## 48. Adding mappings to existing indices

You can add mappings to an existing index using the  `_mapping`  API. If the index already contains data, you will need to reindex the data to apply the new mappings.

Here is an example of how to add mappings to an existing index named  `my_index`  using the Elasticsearch Java API:

```
PutMappingRequest request = new PutMappingRequest("my_index");
request.source("{\"properties\":{\"new_field\":{\"type\":\"text\"}}}", XContentType.JSON);

AcknowledgedResponse response = client.indices().putMapping(request, RequestOptions.DEFAULT);

```

This code sends a  `PUT`  request to the  `_mapping`  API with the name of the index and the new mapping definition. The response indicates whether the operation was successful.

## 49. How dates work in Elasticsearch

In Elasticsearch, dates are stored as a timestamp in milliseconds since the Unix epoch. You can use the  `date`  data type to define  date fields  in your mapping.

Here is an example of how to define a  date field  in a mapping:

```
{
  "mappings": {
    "properties": {
      "timestamp": { "type": "date" }
    }
  }
}

```

This mapping defines a field called  `timestamp`  with a  `date`  data type.

## 50. How  missing fields  are handled

When querying Elasticsearch, you may encounter fields that are missing from some documents. Elasticsearch provides several options for how to handle these missing fields, including throwing an exception, returning a null value, or returning a default value.

Here is an example of how to handle missing fields in a query:

```
{
  "query": {
    "match": {
      "missing_field": {
        "query": "default_value",
        "default_operator": "OR"
      }
    }
  }
}

```

This query searches for documents that contain the  `missing_field`  with a default value of  `default_value`  if the field is missing. The  `default_operator`  parameter specifies how to combine the  default value  with other query terms.

## 51. Overview of  mapping parameters

Elasticsearch provides several mapping parameters that allow you to customize the behavior of your fields, including their data type, indexing options, and storage options. Some common mapping parameters include  `type`,  `index`,  `store`,  `analyzer`, and  `fields`.

Here is an example of how to use mapping parameters to define a field:


```
{
  "mappings": {
    "properties": {
      "my_field": {
        "type": "text",
        "index": true,
        "store": false,
        "analyzer": "standard",
        "fields": {
          "keyword": {
            "type": "keyword",
            "ignore_above": 256
          }
        }
      }
    }
  }
}

```

This mapping defines a field called  `my_field`  with a  `text`  data type,  `standard`  analyzer, and a sub-field called`keyword`  with a  `keyword`  data type and a maximum length of 256 characters.

## 52. Updating existing mappings

You can update an existing mapping in Elasticsearch by sending a  `PUT`  request to the  `_mapping`  API with the new mapping definition. However, some changes to the mapping may require reindexing of the data to take effect.

Here is an example of how to update an existing mapping for a field called  `my_field`  to change its data type from  `text`  to  `keyword`:

```
{
  "properties": {
    "my_field": {
      "type": "keyword"
    }
  }
}

```

This mapping updates the  `my_field`  field to use a  `keyword`  data type instead of  `text`.

## 53. Reindexing documents with the Reindex API

The  Reindex API  in Elasticsearch allows you to copy documents from one index to another, or to modify documents during the copying process. This can be useful when you need to change the mapping or  data structure  of an index.

Here is an example of how to use the Reindex API to reindex documents from an index called  `old_index`  to a new index called  `new_index`:

```
POST _reindex
{
  "source": {
    "index": "old_index"
  },
  "dest": {
    "index": "new_index"
  }
}

```

This request copies all the documents from the  `old_index`  to the  `new_index`. You can also use the Reindex API to modify documents during the copying process by specifying a  `script`  parameter or a  `query`  parameter to filter the documents to be copied.
