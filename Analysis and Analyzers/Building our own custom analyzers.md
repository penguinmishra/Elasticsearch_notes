## Building our own custom analyzers

This can be done by using <code>settings</code> in <code>PUT</code> request while creating an index the below way:
```sh
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
**IMPORTANT : DO NOT INCLUDE <code>standard</code> IN <code>filter</code>. THIS HAS BEEN REMOVED. WHILE ADDING THE INDEX, THIS WON'T THROW ANY EXCEPTION, BUT WHILE ANALYZING, IT SHALL.**<br>

After we execute the above index creation request, we can analyze a text with our own analyzer:<br>
```sh
POST /analyzers_test/_analyze
{
  "analyzer": "my_analyzer",
  "text": "I'm in a mood for drinking the <strong>semi-dry</strong> red wine!"
}
```