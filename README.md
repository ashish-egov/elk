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


## Match Query

The  match query  is one of the most commonly used full text queries in Elasticsearch. It is used to search for a specific word or phrase in a text field. The match query analyzes the text in the field and then searches for the specified word or phrase in the analyzed text.

Here is an example of a match query:

```
{
  "query": {
    "match": {
      "description": "quick brown fox"
    }
  }
}

```

In this example, we are searching for the phrase "quick brown fox" in the "description" field. Elasticsearch will analyze the text in the field and then search for the phrase in the analyzed text.

## Introduction to Relevance Scoring

When performing a  full text search  in Elasticsearch, the documents that match the  search query  are returned in order of relevance. Relevance is calculated based on a  scoring algorithm  that takes into account factors such as the frequency of the  search terms  in the document, the length of the document, and other factors.

The  relevance score  for a document is a number between 0 and 1, with 1 being the most relevant document. By default, Elasticsearch returns documents in order of relevance, with the most relevant documents first.

## Searching Multiple Fields

In Elasticsearch, you can search for text in multiple fields at once using the multi-match query. The multi-match query allows you to specify a list of fields to search in and a  search term. Elasticsearch will then search for the search term in all of the specified fields.

Here is an example of a multi-match query:

```
{
  "query": {
    "multi_match": {
      "query": "quick brown fox",
      "fields": ["title", "description"]
    }
  }
}

```

In this example, we are searching for the phrase "quick brown fox" in both the "title" and "description" fields. Elasticsearch will analyze the text in both fields and then search for the phrase in the analyzed text.

## Phrase Searches

In Elasticsearch, you can search for a specific phrase using the match_phrase query. The match_phrase query searches for a specific sequence of words in a text field. The sequence of words must appear in the text field in the exact order specified in the query.

Here is an example of a match_phrase query:

```
{
  "query": {
    "match_phrase": {
      "description": "quick brown fox"
    }
  }
}

```

In this example, we are searching for the phrase "quick brown fox" in the "description" field. Elasticsearch will search for the exact sequence of words in the field, in the order specified in the query.

Overall, full text queries are a powerful tool for searching for text in Elasticsearch, and the match query, multi-match query, and match_phrase query are some of the most commonly used full text queries.

# Leaf and Compound Queries

In  Elasticsearch, queries can be classified as either leaf queries or compound queries.  Leaf queries  are the simplest type of query and are used to search for a specific term or terms in a field.  Compound queries, on the other hand, are used to combine  multiple leaf queries  or other compound queries to create more complex  search queries.

## Querying with Boolean Logic

Boolean logic  allows you to combine multiple search conditions using  logical operators  such as AND, OR, and NOT. In Elasticsearch, you can use boolean logic to create  complex search queries  by combining multiple leaf queries or compound queries.

Here is an example of a boolean query:

```
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "title": "apple"
          }
        },
        {
          "range": {
            "price": {
              "gte": 1000,
              "lte": 2000
            }
          }
        }
      ],
      "must_not": [
        {
          "match": {
            "description": "refurbished"
          }
        }
      ],
      "should": [
        {
          "match": {
            "brand": "Apple"
          }
        },
        {
          "match": {
            "brand": "Samsung"
          }
        }
      ]
    }
  }
}

```

In this example, we are searching for products with the word "apple" in the "title" field, a price between 1000 and 2000, and a brand of either "Apple" or "Samsung". We are also excluding products that have the word "refurbished" in the "description" field.

## Query Execution  Contexts

In Elasticsearch, queries can be executed in two different contexts: the query context and the filter context. The  query context  is used to score documents based on the relevance of the query, while the  filter context  is used to filter documents based on the query.

Here is an example of a filtered query:

```
{
  "query": {
    "filtered": {
      "query": {
        "match": {
          "title": "apple"
        }
      },
      "filter": {
        "range": {
          "price": {
            "gte": 1000
          }
        }
      }
    }
  }
}

```

In this example, we are searching for products with the word "apple" in the "title" field and a price greater than or equal to 1000. The  `match`  query is executed in the query context to score documents based on the relevance of the query, while the  `range`  filter is executed in the filter context to filter documents based on the query.

## Boosting Query

In Elasticsearch, you can use  query boosting  to give more weight to certain  search criteria. Query boosting allows you to prioritize certain search criteria over others, making it more likely that the most relevant results will appear at the top of the search results.

Here is an example of a boosted query:

```
{
  "query": {
    "bool": {
      "should": [
        {
          "match": {
            "title": {
              "query": "apple",
              "boost": 2
            }
          }
        },
        {
          "match": {
            "description": "apple"
          }
        }
      ]
    }
  }
}

```

In this example, we are searching for products with the word "apple" in either the "title" or "description" field. However, we have given more weight to the "title" field by setting the  `boost`  parameter to 2.

## Disjunction Max (Dis_Max)

Disjunction Max (Dis_Max) is a query that allows you to search for results that match any of the search criteria. It is useful when you want to search for documents that match multiple  search terms, but want to give more weight to certain search criteria.

Here is an example of a Dis_Max query:

```
{
  "query": {
    "dis_max": {
      "queries": [
        {
          "match": {
            "title": "apple"
          }
        },
        {
          "match": {
            "description": "apple"
          }
        }
      ],
      "tie_breaker": 0.3
    }
  }
}

```

In this example, we are searching for products with the word "apple" in either the "title" or "description" field. We have set the  `tie_breaker`  parameter to 0.3, which means that matches in the "description" field will be given less weight than matches in the "title" field.

## Querying Nested Objects

In Elasticsearch, you can search for  nested objects  within a document using nested queries. Nested queries allow you to search for documents that contain a specific nested object that matches certain criteria.

Here is an example of a nested query:

```
{
  "query": {
    "nested":{
      "path": "variants",
      "query": {
        "bool": {
          "must": [
            {
              "match": {
                "variants.color": "red"
              }
            },
            {
              "range": {
                "variants.price": {
                  "gte": 1000
                }
              }
            }
          ]
        }
      }
    }
  }
}

```

In this example, we are searching for products that have a  nested object  in the "variants" field that has a "color" of "red" and a "price" greater than or equal to 1000.

## Nested Inner Hits

In Elasticsearch, you can use  inner hits  to return nested objects that match a search query. Inner hits allow you to retrieve the nested objects that match a  search query, along with their parent documents.

Here is an example of a  nested query  with inner hits:

```
{
  "query": {
    "nested": {
      "path": "variants",
      "query": {
        "bool": {
          "must": [
            {
              "match": {
                "variants.color": "red"
              }
            },
            {
              "range": {
                "variants.price": {
                  "gte": 1000
                }
              }
            }
          ]
        }
      },
      "inner_hits": {}
    }
  }
}

```

In this example, we are searching for products that have a nested object in the "variants" field that has a "color" of "red" and a "price" greater than or equal to 1000. We have included the  `inner_hits`  parameter to retrieve the nested objects that match the search query.

## Nested Fields Limitations

When using nested objects in Elasticsearch, there are some limitations to be aware of. One limitation is that it can be more difficult to update nested objects than non-nested objects. Another limitation is that nested objects can affect the performance of search queries, especially when the nested objects are deeply nested or there are many of them.

It is important to consider the limitations of nested objects when designing your  Elasticsearch index  and querying strategy. If possible, it may be more efficient to denormalize your data and use non-nested objects instead. However, if nested objects are necessary for your use case, Elasticsearch provides powerful tools for querying and managing nested objects.

# Tie Breaker in  Elasticsearch

Tie breaker is an important feature in Elasticsearch that helps to determine the relevance of search results. It is a parameter that can be set in the  search query  to specify how documents with the same score should be ranked. In this article, we will discuss  tie breaker  in detail, including its definition, how it works, and examples of how it can be used.

## What is Tie Breaker?

When Elasticsearch executes a search query, it returns a list of documents that match the query. Each document is assigned a score based on how well it matches the query. The score is calculated by combining various factors such as  term frequency, field length, and document frequency. In some cases, multiple documents may have the same score. In such situations, Elasticsearch uses a tie breaker to determine the order in which the documents are ranked.

Tie breaker is a parameter that can be set in the search query to specify how Elasticsearch should rank documents with the same score. The  tie breaker value  is a number between 0 and 1. A tie breaker value of 0 means that Elasticsearch should ignore the scores and sort the documents based on their internal document ID. A tie breaker value of 1 means that Elasticsearch should use the scores to determine the order in which the documents are ranked.

## How does Tie Breaker work?

To understand how tie breaker works, let's consider an example. Suppose we have a search query that matches three documents, and each document has a score of 0.8. If we set the tie breaker to 0, Elasticsearch will ignore the scores and sort the documents based on their  internal document ID, which is assigned by Elasticsearch when the document is indexed. In this case, the order of the documents will be arbitrary.

On the other hand, if we set the tie breaker to 1, Elasticsearch will use the scores to determine the order in which the documents are ranked. Since all the documents have the same score, Elasticsearch will use the tie breaker value to break the tie. For example, if we set the tie breaker to 0.1, Elasticsearch will add 0.1 to the score of the first document, 0.2 to the score of the second document, and 0.3 to the score of the third document. The new scores will be 0.9, 1.0, and 1.1, respectively. Elasticsearch will then sort the documents based on their new scores, with the third document being ranked first, followed by the second document, and then the first document.

## Examples of Tie Breaker in Elasticsearch

Let's consider some examples of how tie breaker can be used in Elasticsearch.

### Example 1: Sorting by Multiple Fields

Suppose we have an index of products, and we want to sort the products by their price and their popularity. We can use the following query to achieve this:

```
{
    "query": {
        "match_all": {}
    },
    "sort": [
        { "price": { "order": "asc", "mode": "min", "tie_breaker": 0.7 }},
        { "popularity": { "order": "desc", "mode": "max", "tie_breaker": 0.3 }}
    ]
}

```

In this query, we are  sorting  by the  `price`  field in ascending order and using the  `min`  mode to break ties. We are also setting the tie breaker to 0.7, which means that the price will be the primary sorting criterion. If two products have the same price, Elasticsearch will use the popularity field to break the tie. We are using the  `max`  mode for the  popularity field  and setting the tie breaker to 0.3, which means that popularity will be the secondary  sorting criterion.

### Example 2: Boosting Exact Matches

Suppose we have a search query that matches multiple fields, and we want to boost documents that have an exact match in one of the fields. We can use the following query to achieve this:

```
{
    "query": {
        "multi_match": {
            "query": "apple",
            "fields": ["name^3", "description"],
            "type": "best_fields",
            "tie_breaker": 0.5
        }
    }
}

```

In this query, we are using the  `multi_match`  query to search for the term  `apple`  in the  `name`  and  `description`  fields. We are boosting the  `name`  field by a factor of 3 using the  `^3`  syntax. We are also setting the tie breaker to 0.5, which means that documents with an exact match in the  `name`  field will be ranked higher than documents with a partial match in both fields.

### Example 3: Sorting by Distance

Suppose we have an index of restaurants, and we want to sort the restaurants by their distance from a specific location. We can use the following query to achieve this:

```
{
    "query": {
        "match_all": {}
    },
    "sort": [
        {
            "_geo_distance": {
                "location": {
                    "lat": 40.71427,
                    "lon": -74.00597
                },
                "order": "asc",
                "unit": "km",
                "mode": "min",
                "tie_breaker": 0.5
            }
        }
    ]
}

```

In this query, we are sorting by the  `_geo_distance`  field, which is a special field that calculates the distance between two points using the  Haversine formula. We are specifying the location of the point we want to measure the distance from using the  `location`  parameter. We are setting the order to  `asc`, which means that Elasticsearch will sort the documents by their distance in ascending order. We are using the  `min`  mode to break ties and setting the tie breaker to 0.5, which means that documents with a similar distance will be ranked equally.

## Conclusion

Tie breaker is an important feature in Elasticsearch that helps to determine the relevance of search results. It allows us to specify how documents with the same score should be ranked, based on one or more criteria. By using tie breaker, we can create more precise and accurate  search queries  that return the most relevant results to the user.


## Mapping Document Relationships

In Elasticsearch, mapping is the process of defining how documents and their fields are stored and indexed. Mapping is important because it affects the way that Elasticsearch handles queries and how search results are returned. One important aspect of mapping is defining document relationships.

### One-to-one Relationships

In Elasticsearch, a one-to-one relationship can be defined by simply adding a field to the document that contains the related data. For example, if we have a  `user`  and  `address`  document, we can define a one-to-one relationship between them by adding an  `address`  field to the  `user`  document:

```
PUT /users/_doc/1
{
  "name": "John Doe",
  "address": {
    "street": "123 Main St",
    "city": "Anytown",
    "state": "CA",
    "zip": "12345"
  }
}

```

### One-to-many Relationships

In Elasticsearch, a one-to-many relationship can be defined using  nested documents  or parent-child relationships.

#### Nested Documents

Nested documents allow us to define a one-to-many relationship between documents by nesting the related data inside an array. For example, if we have a  `user`  and  `comment`  document, we can define a one-to-many relationship between them using nested documents:

```
PUT /users/_doc/1
{
  "name": "John Doe",
  "comments": [
    {
      "text": "Great article!",
      "date": "2021-09-01"
    },
    {
      "text": "Thanks for sharing!",
      "date": "2021-09-02"
    }
  ]
}

```

#### Parent-Child Relationships

Parent-child relationships allow us to define a one-to-many relationship between documents by creating a parent document and one or more child documents that are related to it. For example, if we have a  `blog`  and  `comment`  document, we can define a parent-child relationship between them:

```
PUT /blogs/_doc/1
{
  "title": "My Blog Post",
  "content": "This is my blog post.",
  "author": "John Doe"
}

PUT /comments/_doc/1?routing=1
{
  "blog_id": "1",
  "text": "Great post!",
  "date": "2021-09-01"
}

PUT /comments/_doc/2?routing=1
{
  "blog_id": "1",
  "text": "Thanks for sharing!",
  "date": "2021-09-02"
}

```

In this example, we have created a  `blog`  document with an ID of 1 and two related  `comment`  documents with IDs 1 and 2. We have specified the  routing parameter  to ensure that the  child documents  are stored on the same shard as the  parent document.

## Adding Documents

Adding documents to Elasticsearch is a simple process that involves sending an HTTP request to the Elasticsearch server. The request must include the document data in  JSON format  and specify the index and type of the document.

### Indexing a Document

To add a document to Elasticsearch, we can use the  `PUT`  method to send an HTTP request to the server with the document data in JSON format. For example, to add a  `product`  document to the  `products`  index, we can use the following request:

```
PUT /products/_doc/1
{
  "name": "Product 1",
  "description": "This is product 1.",
  "price": 9.99,
  "in_stock": true
}

```

In this example, we are adding a  `product`  document with an ID of 1 to the  `products`  index. The document data is specified in  JSON  format inside the request body.

### Updating a Document

To update a document in Elasticsearch, we can use the  `POST`  method to send an HTTP request to the server with the updated  document data  in JSON format. For example, to update the  `name`  field of the  `product`  document with an ID of 1, we can use the following request:


```
POST /products/_doc/1/_update
{
  "doc": {
    "name": "Product 2"
  }
}

```

In this example, we are updating the  `name`  field of the  `product`  document with an ID of 1 to "Product 2". The  `doc`  parameter is used to specify the updated document data in JSON format.

### Deleting a Document

To delete a document in Elasticsearch, we can use the  `DELETE`  method to send an HTTP request to the server with the ID of the document we want to delete. For example, to delete the  `product`  document with an ID of 1, we can use the following request:

```
DELETE /products/_doc/1

```

In this example, we aredeleting the  `product`  document with an ID of 1 from the  `products`  index.

### Bulk Indexing

Bulk indexing is the process of adding multiple documents to Elasticsearch in a single request. This can be more efficient than indexing individual documents, especially when dealing with large datasets.

To  bulk index  documents in Elasticsearch, we can use the  `POST`  method to send an HTTP request to the server with the document data in a specific format. The data must be specified in JSON format, with each document separated by a newline character. For example, to add two  `product`  documents to the  `products`  index, we can use the following request:

```
POST /products/_bulk
{ "index": { "_id": "1" }}
{ "name": "Product 1", "description": "This is product 1.", "price": 9.99, "in_stock": true }
{ "index": { "_id": "2" }}
{ "name": "Product 2", "description": "This is product 2.", "price": 19.99, "in_stock": false }

```

In this example, we are using the  `index`  action to add two  `product`  documents to the  `products`  index. The  `_id`  parameter is used to specify the  ID  of each document. The document data is specified in JSON format, with each document separated by a newline character.


## Adding departments test data

Before we dive into managing relationships between documents in Elasticsearch, we need to create some test data. Let's start by adding some departments to our Elasticsearch index. We will create a new index called "departments" and add some sample data using the following code:

```
PUT /departments
{
  "mappings": {
    "properties": {
      "name": { "type": "text" },
      "description": { "type": "text" }
    }
  }
}

POST /departments/_doc
{
  "name": "Sales",
  "description": "Responsible for selling products and services"
}

POST /departments/_doc
{
  "name": "Marketing",
  "description": "Responsible for promoting products and services"
}

POST /departments/_doc
{
  "name": "Engineering",
  "description": "Responsible for building products and services"
}

```

## Mapping document relationships

To manage relationships between documents in Elasticsearch, we need to use the "join" datatype. The  join datatype  allows us to create parent-child relationships between documents. Let's create a new index called "employees" and add a mapping with a join field to manage the relationship between employees and departments.

```
PUT /employees
{
  "mappings": {
    "properties": {
      "name": {
        "type": "text"
      },
      "department": {
        "type": "join",
        "relations": {
          "employee": "department"
        }
      }
    }
  }
}

```

In the mapping above, we define a "join" field called "department" that has a relation to the "employee" and "department" types. This mapping allows us to create a parent-child relationship between employees and departments.

## Adding documents

Now that we have defined our mapping, let's add some sample data to our "employees" index.

```
POST /employees/_doc
{
  "name": "John Doe",
  "department": {
    "name": "Sales",
    "parent": "department"
  }
}

POST /employees/_doc
{
  "name": "Jane Smith",
  "department": {
    "name": "Marketing",
    "parent": "department"
  }
}

POST /employees/_doc
{
  "name": "Mark Johnson",
  "department": {
    "name": "Engineering",
    "parent": "department"
  }
}

```

In the documents above, we create three employees and associate them with their respective departments using the join field "department".

## Querying by parent ID

One of the most common operations when dealing with parent-child relationships is querying for  child documents  by  parent ID. Let's say we want to find all employees in the "Sales" department. We can use the following query to achieve this:

```
GET /employees/_search
{
  "query": {
    "has_parent": {
      "parent_type": "department",
      "query": {
        "match": {
          "name": "Sales"
        }
      }
    }
  }
}

```

In this query, we use the "has_parent" query to find all employees that have a parent department with the name "Sales". The "parent_type" parameter specifies the  parent document type, and the "query" parameter specifies the query to find the parent document.

## Querying child documents by parent

Sometimes we may need to query child documents and return their parent documents as well. Let's say we want to find all employees and their respective departments. We can use the "has_child" query to achieve this:

```
GET /departments/_search
{
  "query": {
    "has_child": {
      "type": "employee",
      "query": {
        "match_all": {}
      },
      "inner_hits": {}
    }
  }
}

```

In this query, we use the "has_child" query to find all employees and their respective departments. The "type" parameter specifies the  child document type, and the "query" parameter specifies the query to find the child documents. We also use the "inner_hits" parameter to return the  parent document  for each  matching child document.

## Querying parent by child documents

Sometimes we may need to query  parent documents  and return their child documents as well. Let's say we want to find all departments and their respective employees. We can use the "has_child" query to achieve this:

```
GET /departments/_search
{
  "query": {
    "has_child": {
      "type": "employee",
      "query": {
        "match_all": {}
      },
      "inner_hits": {}
    }
  }
}

```

In this query, we use the "has_child" query to find all departments and their respective employees. The "type" parameter specifies the child document type, and the "query" parameter specifies the query to find the child documents. We also use the "inner_hits" parameter to return the child documents for each matching parent document.

## Multi-level relations

Sometimes we may need to model more complex relationships between documents, such as multi-level relationships. Let's say we want to model a  company structure  where each department has a manager. We can use the following mapping to achieve this:

```
PUT /departments
{
  "mappings": {
    "properties": {
      "name": { "type": "text" },
      "description": { "type": "text" },
      "manager": {
        "type": "join",
        "relations": {
          "department": "employee"
        }
      }
    }
  }
}

```

In this mapping, we define a new  join field  called "manager" that has a relation to the "department" and "employee" types. This mapping allows us to create a multi-level relationship between departments, employees, and managers.

## Parent/child inner hits

When querying parent-child relationships, we may need to return both the parent and child documents in the same query. We can use the "inner_hits" parameter to achieve this. Let's say we want to find all departments and their respective employees with the name "John Doe". We can use the following query to achieve this:

```
GET /departments/_search
{
  "query": {
    "has_child": {
      "type": "employee",
      "query": {
        "match": {
          "name": "John Doe"
        }
      },
      "inner_hits": {}
    }
  }
}

```

In this query, we use the "has_child" query to find all employees with the name "John Doe" and their respective departments. We also use the "inner_hits" parameter to return the parent document for each matching child document.

## Terms lookup mechanism

Sometimes we may need to model relationships between documents using external data sources. We can use the "terms" lookup mechanism to achieve this. Let's say we want to associate each employee with a department based on an external CSV file. We can use the following mapping to achieve this:

```
PUT /employees
{
  "mappings": {
    "properties": {
      "name": { "type": "text" },
      "department_id": { "type": "keyword" },
      "department": {
        "type": "join",
        "relations": {
          "employee": "department"
        }
      }
    }
  }
}

```

In this mapping, we define a new field called "department_id" that stores the  department ID  for each employee. We also define a join field called "department" that has a relation to the "employee" and "department" types. We can then use the following query to associate each employee with their respective department based on the "department_id" field:

```
POST /employees/_update_by_query
{
  "script": {
    "source": "ctx._source.department = ['department', params.dept]",
    "lang": "painless",
    "params": {
      "dept": {
        "_id": "sales",
        "_index": "departments"
      }
    }
  },
  "query": {
    "match_all": {}
  }
}

```

In this query, we use the "update_by_query"  API  to update all employee documents. We use a script to set the "department" field to the "sales" department using the "terms" lookup mechanism.

## Join limitations

While join queries can be a powerful tool for managing relationships between documents, they also have some limitations. Join queries can be slow and resource-intensive, especially when dealing with large datasets. Join queries can also complicate indexing and  query performance, as they require additional processing and memory overhead. Finally, join queries can be difficult to scale horizontally, as they require coordination between shards.

## Join field performance considerations

When using  join fields  in Elasticsearch, there are several performance considerations to keep in mind. Join fields can increase the size of the index and require additional memory overhead. Join fields can also impact indexing and query performance, as they require additional processing and memory overhead. Finally, join fields can impact  shard size  and distribution, as they require coordination between shards. To mitigate these performance considerations, it is important to carefully design your  index mapping  and optimize your indexing and  query strategies.


## Document Types

In  Elasticsearch, a document represents a single instance of an entity in a particular index. Each document has a unique ID and it is stored in JSON format. Elasticsearch allows you to define different  document types  within an index to organize your data.

In previous versions of Elasticsearch, you could define multiple document types within a single index. However, starting from version 7.0, only one  document type  is allowed per index.

## Specifying the Result Format

When you query Elasticsearch, it returns the results in  JSON format  by default. However, you can specify a different format using the  `format`  parameter. The following formats are supported:

-   json (default)
-   yaml
-   msgpack
-   csv
-   tsv

Here's an example of how to specify the  result format  as  `csv`:

```
GET /my-index/_search?format=csv
{
  "query": {
    "match_all": {}
  }
}

```

## Source Filtering

Source filtering  allows you to control which fields are returned in the search results. You can either include or exclude fields from the  `_source`  field of the search results.

Here's an example of how to include only the  `title`  and  `author`  fields in the search results:

```
GET /my-index/_search
{
  "_source": ["title", "author"],
  "query": {
    "match_all": {}
  }
}

```

You can also exclude fields from the search results by using the  `exclude`  parameter:

```
GET /my-index/_search
{
  "_source": {
    "excludes": ["description"]
  },
  "query": {
    "match_all": {}
  }
}

```

## Specifying the Result Size

By default, Elasticsearch returns only 10 search results. However, you can specify the number of search results to be returned using the  `size`  parameter.

Here's an example of how to specify the number of search results as 20:

```
GET /my-index/_search
{
  "size": 20,
  "query": {
    "match_all": {}
  }
}

```

## Specifying an Offset

The  `from`  parameter allows you to skip a certain number of search results. This is useful for implementing pagination.

Here's an example of how to skip the first 20 search results and return the next 10:

```
GET /my-index/_search
{
  "from": 20,
  "size": 10,
  "query": {
    "match_all": {}
  }
}

```

## Pagination

To implement pagination, you can use the  `from`  and  `size`  parameters together. For example, to return the second page of search results with 10 items per page:

```
GET /my-index/_search
{
  "from": 10,
  "size": 10,
  "query": {
    "match_all": {}
  }
}

```

## Sorting Results

You can sort the search results based on one or more fields using the  `sort`  parameter. By default, the search results are sorted in ascending order.

Here's an example of how to sort the search results based on the  `date`  field in descending order:

```
GET /my-index/_search
{
  "sort": [
    {
      "date": {
        "order": "desc"
      }
    }
  ],
  "query": {
    "match_all": {}
  }
}

```

## Sorting by Multi-Value Fields

If a field contains multiple values, you can sort the search results based on a specific value using the  `sort`  parameter.

Here's an example of how to sort the search results based on the second value in the  `tags`  field:

```
GET /my-index/_search
{
  "sort": [
    {
      "tags.1": {
        "order": "asc"
      }
    }
  ],
  "query": {
    "match_all": {}
  }
}

```

## Filters

Filters allow you to narrow down the search results based on specific criteria. You can use different types of filters in Elasticsearch, such as term, range, bool, and more.

Here's an example of how to filter the search results based on the  `status`  field:

```
GET /my-index/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "term": {
            "status": "published"
          }
        }
      ]
    }
  }
}
```

## Introduction to Aggregations

Aggregations inElasticsearch allow you to perform  data analysis  on your search results. Aggregations can be used to calculate metrics such as average, sum, min, max, and more, as well as to group data into buckets based on specific criteria.

## Metric Aggregations

Metric aggregations allow you to perform calculations on the values in a field. There are several types of  metric aggregations, including:

-   `avg`: calculates the average of the values in a field
-   `sum`: calculates the sum of the values in a field
-   `min`: finds the minimum value in a field
-   `max`: finds the  maximum value  in a field
-   `stats`: calculates multiple metrics, including min, max, avg, and more

Here's an example of how to perform an  `avg`  aggregation on the  `price`  field:

```
GET /my-index/_search
{
  "aggs": {
    "avg_price": {
      "avg": {
        "field": "price"
      }
    }
  },
  "query": {
    "match_all": {}
  }
}

```

## Introduction to Bucket Aggregations

Bucket aggregations group documents into buckets based on specific criteria. There are several types of  bucket aggregations, including:

-   `terms`:  groups documents  based on the values in a field
-   `date_histogram`: groups documents based on  date intervals
-   `range`: groups documents based on a range of values in a field
-   `histogram`: groups documents based on  numeric intervals
-   `geo_distance`: groups documents based on distance from a geographic point

Here's an example of how to perform a  `terms`  aggregation on the  `category`  field:

```
GET /my-index/_search
{
  "aggs": {
    "category_count": {
      "terms": {
        "field": "category"
      }
    }
  },
  "query": {
    "match_all": {}
  }
}

```

## Document Counts are Approximate

When performing aggregations,  Elasticsearch  returns approximate  document counts  by default. This is because the exact document count can be expensive to calculate and may not be necessary for many use cases. However, you can get more accurate document counts by setting the  `size`  parameter to 0.

```
GET /my-index/_search
{
  "aggs": {
    "category_count": {
      "terms": {
        "field": "category"
      }
    }
  },
  "size": 0,
  "query": {
    "match_all": {}
  }
}

```

## Nested Aggregations

You can nest aggregations within other aggregations to perform more  complex data  analysis. For example, you could group documents by category and then calculate the  average price  for each category.

```
GET /my-index/_search
{
  "aggs": {
    "category_count": {
      "terms": {
        "field": "category"
      },
      "aggs": {
        "avg_price": {
          "avg": {
            "field": "price"
          }
        }
      }
    }
  },
  "query": {
    "match_all": {}
  }
}

```

## Filtering out Documents

You can use the  `filter`  aggregation to exclude documents from an aggregation. For example, you could exclude documents with a price less than 10.

```
GET /my-index/_search
{
  "aggs": {
    "category_count": {
      "terms": {
        "field": "category"
      },
      "aggs": {
        "filtered_price": {
          "filter": {
            "range": {
              "price": {
                "gte": 10
              }
            }
          },
          "aggs": {
            "avg_price": {
              "avg": {
                "field": "price"
              }
            }
          }
        }
      }
    }
  },
  "query": {
    "match_all": {}
  }
}

```

## Defining Bucket Rules with Filters

You can use the  `filters`  aggregation to group documents into multiple buckets based on specific criteria. For example, you could group documents into two buckets based on whether the price is less than or equal to 10.

```
GET /my-index/_search
{
  "aggs": {
    "price_buckets": {
      "filters": {
        "filters": {
          "cheap": {
            "range": {
              "price": {
                "lte": 10
              }
            }
          },
          "expensive": {
            "range": {
              "price": {
                "gt": 10
              }
            }
          }
        }
      }
    }
  },
  "query": {
    "match_all": {}
  }
}

```

## Range Aggregations

The  `range`  aggregation allows you to group documents into buckets based on a range of values in a field. For example, you could group documents into three buckets based onthe  price range: less than 10, between 10 and 20, and greater than 20.

```
GET /my-index/_search
{
  "aggs": {
    "price_ranges": {
      "range": {
        "field": "price",
        "ranges": [
          {
            "to": 10
          },
          {
            "from": 10,
            "to": 20
          },
          {
            "from": 20
          }
        ]
      }
    }
  },
  "query": {
    "match_all": {}
  }
}

```

## Histograms

The  `histogram`  aggregation allows you to group documents into buckets based on numeric intervals. For example, you could group documents into buckets based on the price range: less than 10, between 10 and 20, between 20 and 30, and greater than 30.

```
GET /my-index/_search
{
  "aggs": {
    "price_histogram": {
      "histogram": {
        "field": "price",
        "interval": 10
      }
    }
  },
  "query": {
    "match_all": {}
  }
}

```

## Global Aggregation

The  `global`  aggregation allows you to perform aggregations across all documents in an index or a subset of documents. For example, you could calculate the average price across all documents in an index.

```
GET /my-index/_search
{
  "aggs": {
    "global_stats": {
      "global": {},
      "aggs": {
        "avg_price": {
          "avg": {
            "field": "price"
          }
        }
      }
    }
  },
  "query": {
    "match_all": {}
  }
}

```

## Missing Field Values

The  `missing`  aggregation allows you to group documents that have a missing value in a field.

```
GET /my-index/_search
{
  "aggs": {
    "missing_values": {
      "missing": {
        "field": "category"
      }
    }
  },
  "query": {
    "match_all": {}
  }
}

```

## Aggregating Nested Objects

You can use the  `nested`  aggregation to perform aggregations on  nested objects  in a document. For example, you could calculate the average price of all items in a nested  `items`  object.

```
GET /my-index/_search
{
  "aggs": {
    "nested_items": {
      "nested": {
        "path": "items"
      },
      "aggs": {
        "avg_price": {
          "avg": {
            "field": "items.price"
          }
        }
      }
    }
  },
  "query": {
    "match_all": {}
  }
}
```
