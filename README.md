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
## 54. Defining Field Aliases

-   Field aliases  allow us to define multiple names for the same field in an index.
-   Aliases can be used to make it easier to search for data in a specific field or to apply different settings to the same field.
-   To define a  field alias, we can use the  `aliases`  parameter when creating or updating an index mapping.
-   Here is an example of how to define a field alias:

```
PUT my_index
{
  "mappings": {
    "properties": {
      "message": {
        "type": "text",
        "fields": {
          "keyword": {
            "type": "keyword",
            "ignore_above": 256
          },
          "search": {
            "type": "text",
            "analyzer": "english"
          }
        }
      },
      "msg": {
        "type": "alias",
        "path": "message"
      }
    }
  }
}

```

-   In this example, we define an alias  `msg`  for the field  `message`.
-   We specify the alias using a new field with a type of  `alias`  and a  `path`  parameter that specifies the field to alias.

## 55. Multi-Field Mappings

-   Multi-field mappings allow us to index the same field in different ways for different purposes.
-   For example, we might want to index a field as both a  `text`  field for full-text search and as a  `keyword`  field for aggregations.
-   To define a multi-field mapping, we can use the  `fields`  parameter when creating or updating an index mapping.
-   Here is an example of how to define a multi-field mapping:

```
PUT my_index
{
  "mappings": {
    "properties": {
      "message": {
        "type": "text",
        "fields": {
          "keyword": {
            "type": "keyword",
            "ignore_above": 256
          },
          "search": {
            "type": "text",
            "analyzer": "english"
          }
        }
      }
    }
  }
}

```

-   In this example, we define a  `text`  field  `message`  and two sub-fields:  `message.keyword`  and  `message.search`.
-   The  `message.keyword`  field is a  `keyword`  field that can be used for aggregations and exact matches.
-   The  `message.search`  field is also a  `text`  field, but it uses a different analyzer to support full-text search.

## 56. Index Templates

-   Index templates  allow us to define mappings and settings that will be applied automatically to new indices created that match a certain pattern.
-   For example, we might want to define a template that applies to all indices whose names start with  `logstash-`.
-   To define an  index template, we can use the  `PUT _template/template_name`  API.
-   Here is an example of how to define an index template:

```
PUT _template/my_template
{
  "index_patterns": ["logstash-*"],
  "settings": {
    "number_of_shards": 1
  },
  "mappings": {
    "properties": {
      "message": {
        "type": "text"
      }
    }
  }
}

```

-   In this example, we define an index template  `my_template`  that applies to all indices whose names start with  `logstash-`.
-   The template sets the number of shards to 1 and defines a mapping for the  `message`  field.

## 57. Introduction to the  Elastic Common Schema  (ECS)

-   The Elastic Common Schema (ECS) is a specification that defines a common set of fields for logging and metrics data.
-   The goal of ECS is to make it easier to search and analyze data from different sources by providing a common vocabulary.
-   ECS defines a set of fields for common data types like IP addresses,  URLs, and user agents, as well as fields for metadata like timestamps and  log levels.
-   Here is an example of how to use ECS in an index mapping:

```
PUT my_index
{
  "mappings": {
    "properties": {
      "@timestamp": {
        "type": "date"
      },
      "event": {
        "properties": {
          "category": {
            "type": "keyword"
          },
          "type": {
            "type": "keyword"
          }
        }
      },
      "source": {
        "properties": {
          "ip": {
            "type": "ip"
          },
          "port": {
            "type": "integer"
          }
        }
      }
    }
  }
}

```

-   In this example, we define an  index mapping  that uses  ECS fields  for the  `@timestamp`,  `event`, and  `source`  fields.
-   The  `event`  field has sub-fields for  `category`  and  `type`, while the  `source`  field has sub-fields for  `ip`  and  `port`.
-   By using ECS fields, we can ensure that our data is consistent and easier to search and analyze.

## 58. Introduction to  Dynamic Mapping

-   Dynamic mapping is a feature of  Elasticsearch  that automatically detects and maps new fields as they are indexed.
-   This allows us to index data without defining a mapping beforehand, which can be useful when dealing with unstructured or  changing data.
-   When Elasticsearch encounters a new field, it uses heuristics to determine the data type and other properties of the field.
-   Here is an example of how dynamic mapping works:

```
PUT my_index/_doc/1
{
  "message": "hello world",
  "count": 1,
  "tags": ["red", "green", "blue"]
}

```

-   In this example, we index a document with three fields:  `message`,  `count`, and  `tags`.
-   Since we did not define a mapping beforehand, Elasticsearch will create one automatically based on the data it sees.
-   In this case, Elasticsearch will map  `message`  as a  `text`  field,  `count`  as a  `long`  field, and  `tags`  as a  `keyword`  field.

## 59. Combining Explicit and Dynamic Mapping

-   While dynamic mapping can be useful, it may not always produce the desired results.
-   In some cases, we may want to combine  explicit mapping  for certain fields with dynamic mapping for others.
-   To do this, we can define an explicit mapping for certain fields and allow dynamic mapping for others.
-   Here is an example of how to combine explicit and dynamic mapping:

```
PUT my_index
{
  "mappings": {
    "properties": {
      "message": {
        "type": "text"
      }
    }
  }
}

```

-   In this example, we define an explicit mapping for the  `message`  field as a  `text`  field.
-   Now, when we index a document with a new field, Elasticsearch will use dynamic mapping to determine the data type and other properties of the new field.

## 60. Configuring Dynamic Mapping

-   Dynamic mapping can be configured to control how fields are mapped.
    
-   We can configure dynamic mapping using the  `dynamic`  parameter when defining an index mapping.
    
-   Here are some possible values for the  `dynamic`  parameter:
    
    -   `true`  (default): Allow dynamic mapping for new fields.
    -   `false`: Disable dynamic mapping for new fields.
    -   `strict`: Throw an exception if a new field is encountered that does not match an existing  field mapping.
-   Here is an example of how to configure dynamic mapping:
    

```
PUT my_index
{
  "mappings": {
    "dynamic": "strict",
    "properties": {
      "message": {
        "type": "text"
      }
    }
  }
}

```

-   In this example, we configure dynamic mapping to be strict, which means that Elasticsearch will throw an exception if a new field is encountered that does not match an existing field mapping.

## 61. Dynamic Templates

-   Dynamic templates  allow us to define patterns that match field names and specify how those fields should be mapped.
-   This can be useful when dealing with data that has a large number of fields that follow a consistent  naming convention.
-   To define a  dynamic template, we can use the  `dynamic_templates`  parameter when defining an index mapping.
-   Here is an example of how to use dynamic templates:

```
PUT my_index
{
  "mappings": {
    "dynamic_templates": [
      {
        "strings": {
          "match_mapping_type": "string",
          "mapping": {
            "type": "text"
          }
        }
      },
      {
        "longs": {
          "match_mapping_type": "long",
          "mapping": {
            "type": "scaled_float",
            "scaling_factor": 1000
          }
        }
      }
    ]
  }
}

```

-   In this example, we define two dynamic templates.
-   The first template,  `strings`, matches any field with a  mapping type  of  `string`  and maps it as a  `text`  field.
-   The second template,  `longs`, matches any field with a mapping type of  `long`  and maps it as a  `scaled_float`  field with a  scaling factor  of 1000.

## 62. Mapping Recommendations

When mapping data in  Elasticsearch, it's important to choose the appropriate data types for each field to ensure accurate search results and efficient indexing. Here are some mapping recommendations:

-   Use the  `text`  data type for full-text fields that require analysis, such as descriptions or titles.
-   Use the  `keyword`  data type for fields that don't require analysis, such as  IDs  or categories.
-   Use the  `date`  data type for  date fields.
-   Use the  `integer`,  `long`, or  `double`  data types for numeric fields.
-   Use the  `boolean`  data type for  boolean fields.

Here's an example of how to create a mapping for an index:

```
PUT my-index
{
  "mappings": {
    "properties": {
      "title": {
        "type": "text"
      },
      "category": {
        "type": "keyword"
      },
      "date": {
        "type": "date"
      },
      "views": {
        "type": "integer"
      },
      "published": {
        "type": "boolean"
      }
    }
  }
}

```

## 63. Stemming & Stop Words

Stemming is the process of reducing a word to its base or root form. This can be helpful for  search queries  because it allows for variations of a word to match the same results. For example, if a user searches for "running", they may also want results that include "run" or "runner".

Stop words are common words such as "the" or "and" that are often excluded from search queries because they don't add much meaning to the query. Including  stop words  in search queries can lead to  irrelevant results  and slow down  query performance.

Elasticsearch provides built-in analyzers for stemming and removing stop words, which can be applied to  text fields  in a mapping.

Here's an example of a mapping with a custom analyzer that includes stemming and removes stop words:

```
PUT my-index
{
  "settings": {
    "analysis": {
      "analyzer": {
        "english": {
          "tokenizer": "standard",
          "filter": [
            "lowercase",
            "english_stop",
            "english_stemmer"
          ]
        }
      },
      "filter": {
        "english_stop": {
          "type": "stop",
          "stopwords": "_english_" 
        },
        "english_stemmer": {
          "type": "stemmer",
          "language": "english"
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "title": {
        "type": "text",
        "analyzer": "english"
      }
    }
  }
}

```

## 64. Analyzers and Search Queries

Analyzers are used to break down text into individual tokens that can be searched and analyzed. Elasticsearch provides several built-in analyzers, such as the  `standard`  analyzer, which tokenizes text based on whitespace and punctuation.

When querying text fields, it's important to use the same analyzer that was used during indexing to ensure accurate results. If a different analyzer is used during querying, the  search terms  may not match the  indexed tokens.

Here's an example of a  search query  that uses the same analyzer that was used during indexing:

```
GET my-index/_search
{
  "query": {
    "match": {
      "title": {
        "query": "running shoes",
        "analyzer": "english"
      }
    }
  }
}

```

## 65. Built-in Analyzers

Elasticsearch provides several built-in analyzers that can be used for text fields in a mapping. Here are some of the most commonly used built-in analyzers:

-   `standard`: Tokenizes text based on whitespace and punctuation.
-   `simple`: Tokenizes text based on whitespace and lowercase all tokens.
-   `english`: Includes stemming and removes stop words for  English  text.
-   `whitespace`: Tokenizes text based on whitespace only.
-   `keyword`: Does not tokenize text and indexes the entire field as a single  token.

Here's an example of how to use the  `english`  analyzer for a text field in a mapping:

```
PUT my-index
{
  "mappings": {
    "properties": {
      "title": {
        "type": "text",
        "analyzer": "english"
      }
    }
  }
}

```

## 66. Creating Custom Analyzers

In addition to the built-in analyzers, Elasticsearch allows for the creation of  custom analyzers  with specific tokenizers and filters.

Here's an example of how to create a  custom analyzer  with a  `ngram`  tokenizer and a  `lowercase`  filter:

```
PUT _settings
{
  "analysis": {
    "analyzer": {
      "my_analyzer": {
        "type": "custom",
        "tokenizer": "my_tokenizer",
        "filter": [
          "lowercase"
        ]
      }
    },
    "tokenizer": {
      "my_tokenizer": {
        "type": "ngram",
        "min_gram": 2,
        "max_gram": 4
      }
    }
  }
}

```

This custom analyzer includes a  `ngram`  tokenizer, which breaks down text into all possible combinations of letters within a specified range (in this case, between 2 and 4 characters). The  `lowercase`  filter is then applied to lowercase all tokens.

## 67. Adding Analyzers to Existing Indices

Analyzers can be added to existing indices by updating the index settings with the new analyzer. However, existing documents will not be re-analyzed with the new analyzer. To re-index existing documents with the new analyzer, the index may need to be re-created.

Here's an example of how to update a mapping with a new analyzer:

```
PUT my-index/_settings
{
  "analysis": {
    "analyzer": {
      "my_analyzer": {
        "type": "custom",
        "tokenizer": "my_tokenizer",
        "filter": [
          "lowercase"
        ]
      }
    },
    "tokenizer": {
      "my_tokenizer": {
        "type": "ngram",
        "min_gram": 2,
        "max_gram": 4
      }
    }
  }
}

```

## 68. Updating Analyzers

Analyzers can be updated by modifying the  analyzer settings  in the  index settings  and re-indexing the documents. However, changing the analyzer for a field can have significant impacts on search results and relevancy, so it's important to carefully consider the effects of any changes.

Here's an example of how to update the analyzer for a field:

```
PUT my-index/_mapping
{
  "properties": {
    "title": {
      "type": "text",
      "analyzer": "new_analyzer"
    }
  }
}

```

This updates the  `title`  field to use the  `new_analyzer`  analyzer. After updating the mapping, the documents in the index will need to be re-indexed to apply the new analyzer.


## 70. Introduction to Searching

Searching in  Elasticsearch  is done using queries, which can be run against one or more indices to retrieve matching documents. Queries can be simple or complex, and can use a variety of  search parameters  to find the desired results.

Here's an example of a simple  search query  that retrieves all documents in an index:


```
GET my-index/_search

```

## 71. Introduction to Term Level Queries

Term level queries are used to match  exact terms  or phrases in a field. These queries are useful when searching for specific keywords or phrases, and can be combined with other search parameters to refine the results.

Here are some of the most commonly used term level queries in Elasticsearch:

-   `term`: Matches exact terms in a field.
-   `terms`: Matches multiple exact terms in a field.
-   `match`: Matches terms in a field using a specific analyzer.
-   `match_phrase`: Matches exact phrases in a field.

Here's an example of a  `term`  query that retrieves documents where the  `category`  field contains the term "electronics":

json

Copy

```
GET my-index/_search
{
  "query": {
    "term": {
      "category": "electronics"
    }
  }
}

```

## 72. Searching for Terms

Searching for terms in Elasticsearch can be done using term level queries or full-text queries. Term level queries are used to match exact terms in a field, while full-text queries are used to match terms based on analysis.

Here's an example of a full-text search query that retrieves documents where the  `title`  field contains the term "running shoes":

```
GET my-index/_search
{
  "query": {
    "match": {
      "title": "running shoes"
    }
  }
}

```

This query uses the  `match`  query to search the  `title`  field using the  default analyzer.

## 73. Retrieving Documents by IDs

Retrieving documents by their IDs in Elasticsearch is a fast and efficient way to retrieve specific documents. This can be done using the  `get`  API, which retrieves a document by its ID from a specific index.

Here's an example of how to retrieve a document by its ID:

```
GET my-index/_doc/1234

```

This retrieves the document with the ID  `1234`  from the index  `my-index`.

## 74. Range Searches

Range searches in Elasticsearch are used to retrieve documents where a field falls within a specified range. This can be useful for filtering documents based on numeric or date fields.

Here's an example of a range search query that retrieves documents where the  `price`  field is between 50 and 100:

```
GET my-index/_search
{
  "query": {
    "range": {
      "price": {
        "gte": 50,
        "lte": 100
      }
    }
  }
}

```

This query uses the  `range`  query to search the  `price`  field for values greater than or equal to 50 and less than or equal to 100.

## 75. Prefixes, Wildcards & Regular Expressions

Prefixes, wildcards, and regular expressions can be used in Elasticsearch to search for terms that match a specific pattern.

-   Prefixes:  Match terms  that begin with a specific prefix.
-   Wildcards: Match terms that contain a specific pattern of characters.
-   Regular expressions: Match terms based on a specific pattern of characters using  regular expressions.

Here's an example of a  wildcard search query  that retrieves documents where the  `title`  field contains the term "runners":

```
GET my-index/_search
{
  "query": {
    "wildcard": {
      "title": "*runners*"
    }
  }
}

```

This query uses the  `wildcard`  query to search the  `title`  field for terms that contain the string "runners".

## 76. Querying by Field Existence

Querying by  field existence  in Elasticsearch can be done using the  `exists`  query, which retrieves documents where a specific field exists or does not exist.

Here's an example of an  exists query  that retrieves documents where the  `description`  field exists:

```
GET my-index/_search
{
  "query": {
    "exists": {
      "field": "description"
    }
  }
}

```

This query uses the  `exists`  query to search for documents where the  `description`  field exists. To retrieve documents where the field does not exist, you can use the  `must_not`  parameter:

```
GET my-index/_search
{
  "query": {
    "bool": {
      "must_not": {
        "exists": {
          "field": "description"
        }
      }
    }
  }
}

```

This query uses a  `bool`  query to negate the  `exists`  query and retrieve documents where the  `description`  field does not exist.
