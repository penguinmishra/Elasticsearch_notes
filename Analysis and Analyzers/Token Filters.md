## Token Filters

Works on token streams received from tokenizers.<br>

- `standard`: doesn't do anything. Acts as a placeholder for future versions.

- `lowercase`
- `uppercase`

- `nGram`: Does the same thing as `ngram` tokenizer but using this provides the flexibility to use another tokenizer.<br> `[Red, Wine]` becomes `[R, Re, Red, e, ed, d, W, Wi, i, in, n, ne, e]`.

- `edgeNGram`: Does the same thing as `edge_ngram`. `[Red, Wine]` becomes `[R, Re, W, Wi]` with a settiing of minimum letter 1 and max 2.

- `stop`: removes stop words. ES has a default list for languages which can be provided explicitly. `[I'm, in, the, mood, for, drinking, semi, dry, red, wine]` becomes `[I'm, mood, drinking, semi, dry, red, wine]`

- `word_delimiter`: splits words in subwords based on some rules which can be enabled/disabled as you wish. Splits:
	- every non-alphanumeric character
	- switching cases
	- change from alphabet to numbers or vice-versa
	- apostrophe is removed and immediate s is removed. <br>
`[Wi-Fi, PowerShell, CE1000, Andy's]` become `[Wi, Fi, Power, Shell, CE, 1000, Andy]`

- `stemmer`: reduces words to their base forms so that the search matches regardless of what form the word was searched in. `[I'm, in, the, mood, for, drinking, semi, dry, red, wine]` becomes `[I'm, in, the mood, for , drink, semi, dry, red, wine]`

- `keyword_marker`: protects words from reducing by stemmer filter. Specify the word which should be protected.<br> **Protected term**:\[drinking].<br>`[I'm, in, the, mood, for, drinking, semi, dry, red, wine]` remains `[I'm, in, the, mood, for, drinking, semi, dry, red, wine]`.

- `snowball` : used for stemming. Enables you to use stemming with `snowball` algorithm which is a string processing programming language which implements stemmers. Specify the language that you want to apply stemmer for. Such as English.

- `synonym`: used to handle synonyms. `[I, am, very, happy]` becomes `[I, am, very, happy/delighted]`. Here / is used for representation purpose only. This is not how it is done internally. You need to supply configuration file which has words that have same meaning. Synonyms will be injected as terms. You can write terms that match terms in a specific order and because the order is preserved, that'll still work. <br>

Some usual token filters:<br>
- `trim`: trims any leading/trailing whitespaces. Handled by tokenizers themselves most of the times.
- `length`: removes token that are either too short or too long based on length you specify.
- `truncate`: truncates tokens in max length that you specify.