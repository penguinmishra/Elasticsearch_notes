## Using analyzers in mappings

After creating index, we can provide the analyzers in the mapping the following way:<br>
```
PUT /analyzers_test/_mapping
{
 "properties": {
   "description": {
     "type": "text",
     "analyzer": "my_analyzer"
   },
   "teaser": {
     "type" : "text",
     "analyzer": "standard"
   }
 }
}
```

Now putting a document in it:<br>
```
POST /analyzers_test/_doc/1
{
  "description": "drinking",
  "teaser": "drinking"
}
```

Now for fetching, we do that one by one. First we do that for teaser:<br>
```
GET /analyzers_test/_search
{
  "query": {
    "term": {
      "teaser": {
        "value": "drinking"
      }
    }
  }
}
```
The above gives:<br>
```
{
  "took" : 762,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 0.2876821,
    "hits" : [
      {
        "_index" : "analyzers_test",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : 0.2876821,
        "_source" : {
          "description" : "drinking",
          "teaser" : "drinking"
        }
      }
    ]
  }
}
```
But the below:<br>
```
GET /analyzers_test/_search
{
  "query": {
    "term": {
      "description": {
        "value": "drinking"
      }
    }
  }
}
```
gives:<br>
```
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 0,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  }
}
```
This does not give any result because the my_analyzer is defined as follows and uses `stemmer` filter and saves drinking as drink:<br>
```
PUT /analyzers_test
{
  "settings": {
    "analysis": {
      "analyzer": {
        "english_stop": {
          "type": "standard",
          "stopwords": "_english_"
        },
        "my_analyzer": {
          "type": "custom",
          "char_filter": [ "html_strip" ],
          "tokenizer": "standard",
          "filter": [
            "lowercase",
            "trim",
            "my_stemmer"
          ]
        }
      },
      "filter": {
        "my_stemmer": {
          "type": "stemmer",
          "name": "english"
        }
      }
    }
  }
}
```