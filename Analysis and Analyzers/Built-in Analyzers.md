## Built-in Analyzers

Analyzers are a combination of 3: character filters, tokenizers and token filters,

### Default anayzers

#### <code>standard</code>:

- divides text into terms using Unicode Text Segmentation (word boundaries).
- removes punctuation, lowercase terms & optionally removes stop words.
- combination of standard tokenizer + standard token filter + lowercase token filter + stop token filter (optional).
- <code>"I'm in the mood for drinking semi-dry red wine!"</code> becomes <code>\[i, m, in, the, mood, for, drinking, semi, dry, red, wine]</code>

#### <code>simple</code>

- divides text into terms when encountering a character that is not a letter. Also, lowercases all terms.
- combination of letter and lowercase tokenizer
- <code>"I'm in the mood for drinking semi-dry red wine!"</code> becomes <code>\[i, m , in, the, mood, for, drinking, semi, dry, red, wine]</code>.

#### <code>stop</code>

- simple analyzer + removes stop words. lowercase tokenizer + stop token filter.
- difference between <code>standard</code> + stop & this is of the tokenizer. But the standard is better fit.
- <code>"I'm in the mood for drinking semi-dry red wine!"</code> becomes <code>\[i, m, mood, drinking, semi, dry, red, wine]</code>

### Language Analyzers Group (<code>english,...</code>)

language specific analyzers. Internally uses various filters such as <code>stop</code> token filter and <stemmer> token filter.<br>
Example: <code>"I'm in the mood for drinking semi-dry red wine!"</code> becomes <code>\[i'm, mood, drink, semi, dry, red, wine]</code>

#### <code>keyword</code>

- no-op analyzer which returns the input as a single term. This is done by <code>keyword</code> tokenizer internally.
- <code>"I'm in the mood for drinking semi-dry red wine!"</code> becomes <code>\[I'm in the mood for drinking semi-dry red wine!]</code>

#### <code>pattern</code>

- splits text into token based on supplied regex. Done by <code>pattern</code> tokenizer internally.
- <code>lowercase</code> + <code>stop</code> token filters are optional.
- <code>"I, like, red, wine!"</code> becomes <code>\[I, like, red, wine!]</code> using comma as regex.

#### <code>whitespace</code>

- breaks text into terms when encountering a whitespace character. Does with <code>whitespace</code> tokenizer.
- <code>"I'm in the mood for drinking semi-dry red wine!"</code> becomes <code>\[I'm, in, the, mood, for, drinking, semi-dry, red, wine!]
