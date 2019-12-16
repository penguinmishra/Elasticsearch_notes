## Token Filters

Works on token streams received from tokenizers.<br>

- <code>standard</code>: doesn't do anything. Acts as a placeholder for future versions.

- <code>lowercase</code>
- <code>uppercase</code>

- <code>nGram</code>: Does the same thing as <code>ngram</code> tokenizer but using this provides the flexibility to use another tokenizer.<br> <code>\[Red, Wine]</code> becomes <code>\[R, Re, Red, e, ed, d, W, Wi, i, in, n, ne, e]</code>.

- <code>edgeNGram</code>: Does the same thing as <code>edge_ngram<code>. <code>\[Red, Wine] becomes <code>\[R, Re, W, Wi]</code> with a settiing of minimum letter 1 and max 2.

- <code>stop</code>: removes stop words. ES has a default list for languages which can be provided explicitly. <code>\[I'm, in, the, mood, for, drinking, semi, dry, red, wine]</code> becomes <code>\[O'm, mood, drinking, semi, dry, red, wine]</code>

- <code>word_delimiter</code>: splits words in subwords based on some rules which can be enabled/disabled as you wish. Splits:
	- every non-alphanumeric character
	- switching cases
	- change from alphabet to numbers or vice-versa
	- apostrophe is removed and immediate s is removed. <br>
<code>\[Wi-Fi, PowerShell, CE1000, Andy's]</code> become <code>\[Wi, Fi, Power, Shell, CE, 1000, Andy]</code>

- <code>stemmer</code>: reduces words to their base forms so that the search matches regardless of what form the word was searched in. <code>\[I'm, in, the, mood, for, drinking, semi, dry, red, wine]</code> becomes <code>\[I'm, in, the mood, for , drink, semi, dry, red, wine]</code>

- <code>keyword_marker</code>: protects words from reducing by stemmer filter. Specify the word which should be protected.<br> **Protected term**:\[drinking].<br><code>\[I'm, in, the, mood, for, drinking, semi, dry, red, wine]</code> remains <code>\[I'm, in, the, mood, for, drinking, semi, dry, red, wine]</code>.

- <code>snowball</code> : used for stemming. Enables you to use stemming with <code>snowball</code> algorithm which is a string processing programming language which implements stemmers. Specify the language that you want to apply stemmer for. Such as English.

- <code>synonym</code>: used to handle synonyms. <code>\[I, am, very, happy]</code> becomes <code>\[I, am, very, happy/delighted]</code>. Here / is used for representation purpose only. This is not how it is done internally. You need to supply configuration file which has words that have same meaning. Synonyms will be injected as terms. You can write terms that match terms in a specific order and because the order is preserved, that'll still work. <br>

Some usual token filters:<br>
- <code>trim</code>: trims any leading/trailing whitespaces. Handled by tokenizers themselves most of the times.
- <code>length</code>: removes token that are either too short or too long based on length you specify.
- <code>truncate</code>: truncates tokens in max length that you specify.