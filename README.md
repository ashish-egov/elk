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

The response will contain information about the update operation, including the document ID and index name. The script updates the "description" field with the new value provided in the "params" section.
