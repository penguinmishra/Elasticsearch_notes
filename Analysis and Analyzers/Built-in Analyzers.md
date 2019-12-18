## Built-in Analyzers

Analyzers are a combination of 3: character filters, tokenizers and token filters,

### Default anayzers

#### `standard`:

- divides text into terms using Unicode Text Segmentation (word boundaries).
- removes punctuation, lowercase terms & optionally removes stop words.
- combination of standard tokenizer + standard token filter + lowercase token filter + stop token filter (optional).
- `"I'm in the mood for drinking semi-dry red wine!"` becomes `\[i, m, in, the, mood, for, drinking, semi, dry, red, wine]`

#### `simple`

- divides text into terms when encountering a character that is not a letter. Also, lowercases all terms.
- combination of letter and lowercase tokenizer
- `"I'm in the mood for drinking semi-dry red wine!"` becomes `\[i, m , in, the, mood, for, drinking, semi, dry, red, wine]`.

#### `stop`

- simple analyzer + removes stop words. lowercase tokenizer + stop token filter.
- difference between `standard` + stop & this is of the tokenizer. But the standard is better fit.
- `"I'm in the mood for drinking semi-dry red wine!"` becomes `\[i, m, mood, drinking, semi, dry, red, wine]`

### Language Analyzers Group (`english,...`)

language specific analyzers. Internally uses various filters such as `stop` token filter and <stemmer> token filter.<br>
Example: `"I'm in the mood for drinking semi-dry red wine!"` becomes `\[i'm, mood, drink, semi, dry, red, wine]`

#### `keyword`

- no-op analyzer which returns the input as a single term. This is done by `keyword` tokenizer internally.
- `"I'm in the mood for drinking semi-dry red wine!"` becomes `\[I'm in the mood for drinking semi-dry red wine!]`

#### `pattern`

- splits text into token based on supplied regex. Done by `pattern` tokenizer internally.
- `lowercase` + `stop` token filters are optional.
- `"I, like, red, wine!"` becomes `\[I, like, red, wine!]` using comma as regex.

#### `whitespace`

- breaks text into terms when encountering a whitespace character. Does with `whitespace` tokenizer.
- `"I'm in the mood for drinking semi-dry red wine!"` becomes `\[I'm, in, the, mood, for, drinking, semi-dry, red, wine!]
