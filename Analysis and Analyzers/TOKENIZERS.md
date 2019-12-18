```
POST /_analyze
{
  "tokenizer": "ngram",
  "filter": ["lowercase"], 
  "char_filter": ["html_strip"], 
  "text":"I'm in the mood to drink wine!"
}
```
## TOKENIZERS
- Word Oriented Tokenizers
- Partial Word Tokenizers
- Structured Text Tokenizers

#### Word Oriented Tokenizers
- `standard`: full text to words. removes symbols (not apostrophe).
```
POST /_analyze
{
  "tokenizer": "standard",
  "text":"I'm in the mood to drink wine!"
}
```
- `letter`: divides text into terms when encountering a character that is not a letter.
```
POST /_analyze
{
  "tokenizer": "letter",
  "text":"I'm in the mood to drink wine!"
}
```
- `lowecase`: works as `letter` but lowercases all terms. removes special characters.
```
POST /_analyze
{
  "tokenizer": "lowercase",
  "text":"I'm in the mood to drink wine!"
}
```

- `whitespace`: divides text into terms when encountering whitespace character. does not remove special characters.
```
POST /_analyze
{
  "tokenizer": "whitespace",
  "text":"I'm in the mood to drink wine!"
}
```
- `uax_url_email`: like standard. but URLs and emails are in single token. Does not remove special characters.
```
POST /_analyze
{
  "tokenizer": "whitespace",
  "text":"Contact us on infor@info.com or visit https:\//www.infor.info.com!"
}
```

#### Partial Word Tokenizer

breaks up text/words into smaller fragments used for partial word matching.

- `ngram`: breaks text into words when encountering certain characters and then emits N-gram of specified length.
```
POST /_analyze
{
  "tokenizer": "ngram",
  "text":"Red Wine"
}
```
gives
```
[Re, Red, ed, Wi, Win, Wine, ine, ne]
```

- `edge_ngram`: emits ngrams of each terms beginning from the start of the word. No great performance when used in auto-completion. Suggestors are better.
```
POST /_analyze
{
  "tokenizer": "edge_ngram",
  "text":"Red Wine"
}
```

#### Structured Text Tokenizers
used for structured text (emails, addresses, zip code, identifiers)

- `keyword`: no-op tokenizer which outputs the exact same text as a single term. Useful when you don't want to use standard tokenizer and want to work with character filters or token filters.
"I'm in the mood to drink wine!" gives `[I'm in the mood to drink wine!]`

- `pattern`: regex separator. Alternatively captures matched text as terms.
"I, like, red, wine!" gives `[I, like, red, wine!]` when regex is a comma.

- `path_hierarchy`: splits hierarchical values (eg system path) and emits a term for each component in the tree. Uses path separator (/) as default splitter which can be configured.
"/path/to/some/directory" becomes \[\/path, \/path/to, \/path/to/some, \/path/to/some/directory]
