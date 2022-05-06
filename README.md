# Understanding ES
## Intro Http and Rest

Anatomy of a Http Request
- METHOD: The verb of the request. GET, POST, PUT, DELETE etc
- PROTOCOL: What flavor of HTTP (1.0, 1.1 etc)
- HOST: What web server you want to talk to
- URL: What resource is being requested
- BODY: Extra data needed by the server
- HEADERS: User-agent, content-type etc.

Why Rest?
> Language and system independent

Curl Command
> curl -H "Content-Type: application/json" <URL> -d '<BODY>'

***Mapping.json File***

This is the file we declare the structure of an index. Think it like a ddl file in rdbms.

***Define MappingFile in ES***

`curl -H "Content-Type: application/json" -XPUT 127.0.0.1:9200/<index-name> --data-binary @<mapping.json file>`

***Load Data into ES***

`curl -H "Content-Type: application/json" -XPOST 127.0.0.1:9200/<index-name>/_bulk --data-binary @<json-file>`

***Search for a Spesific Doc***
```
curl -H "Content-Type: application/json" -XGET '127.0.0.1:9200/<index-name>/_search?pretty' -d '
{
"query": {
"match_phrase": {
"text_entry": "to be or not to be"
}
}
}'
```

## Elasticsearch Overview
- Started off as scalable Lucene
- Horizontally scalable search engine
- Each shard is an inverted index of documents
- Not just for full text search
- Can handle structured data, and can aggregate data quickly

***Kibana***

- Web ui for searching and visualizing
- Often used for log analyzing

***Logstash/Beats***

- Push data into ES

***X-Pack***

Advanced features from elastic co 

- Security, Alerting, Monitoring, Reporting

### Beats vs Logstash
- Both can used to send data to ElasticSearch
- Beats is an agent that can run on the data sources. Its lightweight.
- Logstash can work distributed. Its more heavyweight.

### Logstash
Its not useful for sending data to ElasticSearch. It can send data to lots of system. Its a data processing pipeline

## Concepts
***Documents***

- Things you're searching for. They can be more than text-any structured JSON
- Every document has a unique ID and a type

***Indices***

- Collections of documents
- Contain inverted indices that let you search across everything within them at once, and mappings that define schemas for the data within

For simplicity, think that indices are like database tables and documents are like rows in that tables.

## Inverted Index

Lets say that we have two documents
- Doc 1: the things get. happy, more, more
- Doc 2: more, happy, like

Inverted index lowercases the words for simplicity and then stores which documents have these words

For instance:  
- more: 1,2
- happy: 1,2
- like: 2

It's not quite simple of course. This just gives an overview.
Let's just go a little bit deeper

The scoring is maybe the more game changing feature between SQL databases and Elasticsearch.

- Elasticsearch returns response ordered by relevance score
- Score is calculated with the TF/IDF algorithm
- Scoring is very powerful and customizable 


- `TF-IDF` means Term Frequency * Inverse Document Frequency
- `Term Frequency` is how often a term appears in a given document
- `Document Frequency` is how often a term appears in all documents 
- `Term Frequency / Document Frequency` measures the relevance of a term in a document

For more detail. [See](https://www.elastic.co/guide/en/elasticsearch/guide/current/practical-scoring-function.html)

## What is new in ES7?
- The concept of document types is deprecated
- Es SQL is production ready
- Lots of changes to defaults
- Lucene 8 under the hood
- Bundled Java runtime
- Cross cluster replication is production ready
- Index Lifecycle Management

## Elasticsearch Scalability
- Documents are hashed to a particular shard
- Each shard may be on a different node in a cluster
- Every shard is a self-contained Lucene index of its own

Lets say that, an index has two primary shards and two replicas
- Write requests are routed to the primary shards, then replicated
- Read requests are routed to the primary or any replica

We can't change the number of primary shards 
- Worst case we need to re-index the data
- We can add more replica shards - more read throughput
