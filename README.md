## Beats vs Logstash
- Both can used to send data to ElasticSearch
- Beats is an agent that can run on the data sources. Its lightweight.
- Logstash can work distributed. Its more heavyweight.

## Logstash
Its not useful for sending data to ElasticSearch. It can send data to lots of system. Its a data processing pipeline

## Starter Queries
Pattern is ***api***/***command***
- GET /_cluster/health, buraya curl karsılıklarını yaz
- GET /_cat/nodes?v
- GET /_cat/indices?v
