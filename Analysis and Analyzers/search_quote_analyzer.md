*query wrapped in quotes it is detected as a phrase query*

## `search_quote_analyzer`

The `search_quote_analyzer` setting allows you to specify an analyzer for phrases, this is particularly useful when dealing with disabling stop words for phrase queries.<br><br>
To disable stop words for phrases a field utilising three analyzer settings will be required:

- An `analyzer` setting for indexing all terms including stop words
- A `search_analyzer` setting for non-phrase queries that will remove stop words
- A `search_quote_analyzer` setting for phrase queries that will not remove stop words

```
PUT my_index
{
   "settings":{
      "analysis":{
         "analyzer":{
            "my_analyzer":{ 
               "type":"custom",
               "tokenizer":"standard",
               "filter":[
                  "lowercase"
               ]
            },
            "my_stop_analyzer":{ 
               "type":"custom",
               "tokenizer":"standard",
               "filter":[
                  "lowercase",
                  "english_stop"
               ]
            }
         },
         "filter":{
            "english_stop":{
               "type":"stop",
               "stopwords":"_english_"
            }
         }
      }
   },
   "mappings":{
       "properties":{
          "title": {
             "type":"text",
             "analyzer":"my_analyzer", 
             "search_analyzer":"my_stop_analyzer", 
             "search_quote_analyzer":"my_analyzer" 
         }
      }
   }
}

PUT my_index/_doc/1
{
   "title":"The Quick Brown Fox"
}

PUT my_index/_doc/2
{
   "title":"A Quick Brown Fox"
}

GET my_index/_search
{
   "query":{
      "query_string":{
         "query":"\"the quick brown fox\"" 
      }
   }
}
```
- `my_analyzer` analyzer which tokens all terms including stop words<br>
- `my_stop_analyzer` analyzer which removes stop words<br>
- `analyzer` setting that points to the `my_analyzer` analyzer which will be used at index time<br>
- `search_analyzer` setting that points to the `my_stop_analyzer` and removes stop words for non-phrase queries<br>
- `search_quote_analyzer` setting that points to the `my_analyzer` analyzer and ensures that stop words are not removed from phrase queries<br><br>
- Since the query is wrapped in quotes it is detected as a phrase query therefore the `search_quote_analyzer` kicks in and ensures the stop words are not removed from the query. The `my_analyzer analyzer` will then return the following tokens `[the, quick, brown, fox]` which will match one of the documents. Meanwhile term queries will be analyzed with the `my_stop_analyzer` analyzer which will filter out stop words. So a search for either `The quick brown fox` or `A quick brown fox` will return both documents since both documents contain the following tokens `[quick, brown, fox]`. Without the `search_quote_analyzer` it would not be possible to do exact matches for phrase queries as the stop words from phrase queries would be removed resulting in both documents matching.