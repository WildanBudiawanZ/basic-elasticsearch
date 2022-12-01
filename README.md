# Learn Elastic Search Basic

### Build and Start Container
```
docker compose up -d
```

### Nodes Monitor
```
http://localhost:9201/_cat/nodes?v
```

### Index Check
```
http://localhost:9201/_cat/indice?v
```

### Shard Check
```
localhost:9201/_cat/shards?v
```



# DSL

## create index

https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html

```PUT localhost:9201/belajar_elasticsearch

PUT localhost:9201/belajar_elasticsearch
{
  "settings": {
    "index": {
      "number_of_shards": 5,  
      "number_of_replicas": 2 
    }
  }
}


PUT localhost:9201/my_index
{
  "settings": {
    "index": {
      "number_of_shards": 3,  
      "number_of_replicas": 1 
    }
  }
}
```

## CRUD Data
### Create/Update Data
https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html
```
PUT localhost:9201/my_index/_doc/1
{
    "first_name" : "John",
    "last_name" : "Doe",
    "address" : {
        "city" : "Bandung",
        "state" : "Jawa Barat"
    },
    "hoby" : [ "game", "music", "football" ]
}


GET localhost:9201/my_index/_doc/1
```

### Delete Data
```
DELETE localhost:9201/my_index/_doc/7
```

### Partial Update
https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-update.html
```
POST localhost:9201/my_index/_update/1
{
    "doc" : {
        "first_name" : "Jacob"
    }
}
```

### Search
https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query.html
```
--Match Simple
GET localhost:9201/my_index/_search
{
  "query": {
    "match": {
      "first_name": "John"
    }
  }
}

--Match to embeded
GET localhost:9201/my_index/_search
{
  "query": {
    "match": {
      "address.city": "Bandung"
    }
  }
}

--Match to array
GET localhost:9201/my_index/_search
{
  "query": {
    "match": {
      "hoby": "football"
    }
  }
}

--Multiple boolean condition
GET localhost:9201/my_index/_search
{
  "query": {
      "bool" : {
          "must_not" : {
              "match": {
                  "hoby": "football"
                  }
          }
      }
  }
}
```

https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-bool-query.html
```
--Multiple boolean condition
GET localhost:9201/my_index/_search
{
  "query": {
      "bool" : {
          "must" : [ {
              "match": {
                  "hoby": "football"
                  }
              },
              {
                  "match": {
                      "first_name": "john"
                  }
              }
          ]
      }
  }
}
```

## Aggregations
https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html
```
--Single Aggregate
GET localhost:9201/my_index/_search
{
  "query" : {
      "bool" : {
          "must" : [
          ]
      }
  },
  "aggs" : {
    "hoby" : {
      "terms" : {
        "field" : "hoby.keyword"
      }
    }
  }
}


--Multiple Aggregate
GET localhost:9201/my_index/_search
{
  "query" : {
      "bool" : {
          "must" : [
          ]
      }
  },
  "aggs" : {
    "hoby" : {
      "terms" : {
        "field" : "hoby.keyword"
      }
    },
    "cities" : {
      "terms" : {
        "field" : "address.city"
      }
    }
  }
}
```
