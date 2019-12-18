## Configuring built-in analyzers and token filters

Usually, the analyzers and filters can be specified while creating index using `settings` tag.

```sh
PUT /existing_analyzer_config
{
  "settings": {
    "analysis": {
      "analyzer": {
        "english_stop": {
          "type": "standard",
          "stopwords": "_english_"
        }
      },
      "filter": {
        "my_filter": {
          "type": "stemmer",
          "name": "english"
        }
      }
    }
  }
}
```
#### Using custom analyzer and filter

To use the custom analyzer (english_stop), we need to put a `POST` request:<br>
```sh
POST /existing_analyzer_config/_analyze
{
  "analyzer": "english_stop",
  "text": "I'm in a mood for drinking the semi-dry red wine!"
}
```
This would remove all the stop words from the text.<br><br>

To use custom filter (my_filter), we need to specify the tokenizer and then our filter:<br>
```sh
POST /existing_analyzer_config/_analyze
{
  "tokenizer": "standard", 
  "filter": [ "my_filter" ],
  "text": "I'm in a mood for drinking the semi-dry red wine!"
}
```

This would stem all the words in one form.