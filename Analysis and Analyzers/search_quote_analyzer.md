*query wrapped in quotes it is detected as a phrase query*

## <code>search_quote_analyzer</code>

The <code>search_quote_analyzer</code> setting allows you to specify an analyzer for phrases, this is particularly useful when dealing with disabling stop words for phrase queries.<br><br>
To disable stop words for phrases a field utilising three analyzer settings will be required:

- An <code>analyzer</code> setting for indexing all terms including stop words
- A <code>search_analyzer</code> setting for non-phrase queries that will remove stop words
- A <code>search_quote_analyzer</code> setting for phrase queries that will not remove stop words

```sh
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
- <code>my_analyzer</code> analyzer which tokens all terms including stop words<br>
- <code>my_stop_analyzer</code> analyzer which removes stop words<br>
- <code>analyzer</code> setting that points to the <code>my_analyzer</code> analyzer which will be used at index time<br>
- <code>search_analyzer</code> setting that points to the <code>my_stop_analyzer</code> and removes stop words for non-phrase queries<br>
- <code>search_quote_analyzer</code> setting that points to the <code>my_analyzer</code> analyzer and ensures that stop words are not removed from phrase queries<br><br>
- Since the query is wrapped in quotes it is detected as a phrase query therefore the <code>search_quote_analyzer</code> kicks in and ensures the stop words are not removed from the query. The <code>my_analyzer analyzer</code> will then return the following tokens <code>\[the, quick, brown, fox]</code> which will match one of the documents. Meanwhile term queries will be analyzed with the <code>my_stop_analyzer</code> analyzer which will filter out stop words. So a search for either <code>The quick brown fox</code> or <code>A quick brown fox</code> will return both documents since both documents contain the following tokens <code>\[quick, brown, fox]</code>. Without the <code>search_quote_analyzer</code> it would not be possible to do exact matches for phrase queries as the stop words from phrase queries would be removed resulting in both documents matching.