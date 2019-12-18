## Adding analyzers to existing indices

This can be done with the following way:
- close the index : blocks reads/writes. if search while closed, there would be an error saying that the index is closed.
- add the analyzer(s)
- open the index

#### Closing the index:
```sh
POST /analyzers_test/_close
```
gives:
```sh
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "indices" : {
    "analyzers_test" : {
      "closed" : true
    }
  }
}
```
#### add analyzer(s):

This can be done with <code>\_settings</code> API:
```sh
PUT /analyzers_test/_settings
{
  "analysis":{
    "analyzer": {
      "french_stop":{
        "type":"standard",
        "stopwords":"_french_"
      }
    }
  }
}
```
This gives:
```sh
{
  "acknowledged" : true
}
```
And thus, the analyzer has been added to the index.

#### Open the index:
This is can be done using simply:
```sh
POST /analyzers_test/_open
```
which gives:
```sh
{
  "acknowledged" : true,
  "shards_acknowledged" : true
}
```

## **IMPORTANT**
Since the index has default mapping, it worked but if we were to use this analyzer for an existing MAPPING, we'd have to delete the index and reindex the documents.<br>
However, while specifying the mappings, the analyzer can be provided.<br>
The following example has been taken from [here](ESURL)
```sh
PUT /my_index
{
  "mappings": {
    "properties": {
      "text": { 
        "type": "text",
        "fields": {
          "english": { 
            "type":     "text",
            "analyzer": "english"
          }
        }
      }
    }
  }
}

GET my_index/_analyze 
{
  "field": "text",
  "text": "The quick Brown Foxes."
}

GET my_index/_analyze 
{
  "field": "text.english",
  "text": "The quick Brown Foxes."
}
```
The first <code>GET</code> returns the tokens: <code>\[ the, quick, brown, foxes ]</code>.
The second <code>GET</code> returns the tokens: <code>\[ quick, brown, fox ]</code>.

[ESURL]: <https://www.elastic.co/guide/en/elasticsearch/reference/current/analyzer.html>