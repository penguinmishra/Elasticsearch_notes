```
POST /_analyze
{
  "tokenizer": "ngram",
  "filter": ["lowercase"], 
  "char_filter": ["html_strip"], 
  "text":"I'm in the mood to drink wine!"
}
```
## Character Filters <code>char_filter</code>

#### <code>html_strip</code>
strips html content (tags and signs)

#### <code>mapping</code>
replaces some text or signs based on a mapping.
example: I'm :) for being :( is :( can be filtered to I'm \_happy_ for being \_sad_ is \_sad_

#### <code>pattern_replace</code>
does a regex matching and replaces. Capture group may be used.<br>
Pattern: ([a-zA-Z0-9]+)(-?)<br>
Replacement: $1 (this is a capture group. normal text can be used)<br>
"9c4455-djorf939-dhih348-uyro"<br> becomes
"9c4455djorf939dhih348uyro"
