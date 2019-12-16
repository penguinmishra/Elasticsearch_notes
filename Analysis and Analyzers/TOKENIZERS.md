```sh
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
- <code>standard</code>: full text to words. removes symbols (not apostrophe).
```sh
POST /_analyze
{
  "tokenizer": "standard",
  "text":"I'm in the mood to drink wine!"
}
```
- <code>letter</code>: divides text into terms when encountering a character that is not a letter.
```sh
POST /_analyze
{
  "tokenizer": "letter",
  "text":"I'm in the mood to drink wine!"
}
```
- <code>lowecase</code>: works as <code>letter</code> but lowercases all terms. removes special characters.
```sh
POST /_analyze
{
  "tokenizer": "lowercase",
  "text":"I'm in the mood to drink wine!"
}
```

- <code>whitespace</code>: divides text into terms when encountering whitespace character. does not remove special characters.
```sh
POST /_analyze
{
  "tokenizer": "whitespace",
  "text":"I'm in the mood to drink wine!"
}
```
- <code>uax_url_email</code>: like standard. but URLs and emails are in single token. Does not remove special characters.
```sh
POST /_analyze
{
  "tokenizer": "whitespace",
  "text":"Contact us on infor@info.com or visit https:\//www.infor.info.com!"
}
```

#### Partial Word Tokenizer

breaks up text/words into smaller fragments used for partial word matching.

- <code>ngram</code>: breaks text into words when encountering certain characters and then emits N-gram of specified length.
```sh
POST /_analyze
{
  "tokenizer": "ngram",
  "text":"Red Wine"
}
```
gives
```sh
[Re, Red, ed, Wi, Win, Wine, ine, ne]
```

- <code>edge_ngram</code>: emits ngrams of each terms beginning from the start of the word. No great performance when used in auto-completion. Suggestors are better.
```sh
POST /_analyze
{
  "tokenizer": "edge_ngram",
  "text":"Red Wine"
}
```

#### Structured Text Tokenizers
used for structured text (emails, addresses, zip code, identifiers)

- <code>keyword</code>: no-op tokenizer which outputs the exact same text as a single term. Useful when you don't want to use standard tokenizer and want to work with character filters or token filters.
"I'm in the mood to drink wine!" gives \[I'm in the mood to drink wine!]

- <code>pattern</code>: regex separator. Alternatively captures matched text as terms.
"I, like, red, wine!" gives \[I, like, red, wine!] when regex is a comma.

- <code>path_hierarchy</code>: splits hierarchical values (eg system path) and emits a term for each component in the tree. Uses path separator (/) as default splitter which can be configured.
"/path/to/some/directory" becomes \[\/path, \/path/to, \/path/to/some, \/path/to/some/directory]
