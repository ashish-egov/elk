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

## 30. Optimistic concurrency control

-   Optimistic concurrency control is a technique for handling concurrent updates to a document.
-   Elasticsearch provides a version parameter that can be used to implement optimistic concurrency control.
-   When updating a document, you can specify the current version number, and Elasticsearch will only update the document if the  current version number  matches the one in the index.

Example:


```
PUT /my_index/_doc/1?version=1
{
  "title": "Updated Elasticsearch Course",
  "description": "Learn how to use Elasticsearch and Kibana for search and analytics"
}

```

In this example, Elasticsearch will only update the document if the  current version  number is "1". If the version number has been incremented by another update operation, Elasticsearch will return a  version conflict error.

```

The response will contain information about the update operation, including the document ID and index name. The script updates the "description" field with the new value provided in the "params" section.
