# markdown-it-phon

[![Build Status](https://img.shields.io/travis/Crissov/markdown-it-phon/master.svg?style=flat)](https://travis-ci.org/Crissov/markdown-it-phon)
[![NPM version](https://img.shields.io/npm/v/markdown-it-phon.svg?style=flat)](https://www.npmjs.org/package/markdown-it-phon)
[![Coverage Status](https://coveralls.io/repos/Crissov/markdown-it-phon/badge.svg?branch=master&service=github)](https://coveralls.io/github/Crissov/markdown-it-phon?branch=master)

> Linguistic plugin for [markdown-it](https://github.com/markdown-it/markdown-it) parser, transcribing /phonemic/ and [phonetic] notation to Unicode IPA

## Install

### node.js

```` bash
npm install markdown-it-phon --save
````

### Browser

```` bash
bower install markdown-it-phon --save
````

## Use

```` markdown
foo //bar// /[baz]/ /quuz/ [lorem] [/ipsum/]
````

is turned (with default settings) into

```` html
<p>foo <span class="phonemic ipa" lang="unk">/bar/</span> <span class="phonetic ipa" lang="unk">[baz]</span> <span class="ipa" lang="unk">quuz</span> <span class="ipa" lang="unk">[lorem]</span> <span class="ipa" lang="unk">[ipsum]</span></p>
````

### Init

```` js
var md = require('markdown-it')();
var phon = require('markdown-it-phon');

md.use(phon [, options]);
````

Options are not mandatory:

- __lang__ (String) -- a BCP47 language code, defaults to `unk` for _unknown_
- __from__ (String) -- source notation system, defaults to `z-sampa` which is a superset of `x-sampa`
  - possible values: `sampa | x-sampa | z-sampa | kirshenbaum | apie | worldbet | arpabet | timit | teuthonista | napa | rfe | epa | upa | iapa | extipa | voqs | ipa` 
- __to__ (String) -- target notation system, defaults to `ipa`
  - possible values: `ipa | same`
- __phonemic__ (Array) -- open and close _bracket_ for output
  - default: `[ "/", "/" ]`
- __phonetic__ (Array) -- open and close _bracket_ for output
  - default: `[ "[", "]" ]`
- __start__ (String) -- defaults to `/`
- __end__ (String) -- defaults to `/`


_Differences in browser._ If you load the script directly into the page without
using a package system, the module will add itself globally with the name `markdownitPhon`.
Init code will look a bit different in this case:

```` js
var md = window.markdownit().use(window.markdownitPhon);
````

### change output

By default, IPA is rendered as appropriate unicode chars, 
but you can change the renderer function as you wish.

Render as `i` blocks:

```` js
// ...
// initialize

md.renderer.rules.phon = function(token, idx) {
  return '<i class="ipa">' + token[idx].markup + '</i>';
};
````

Or use a custom transliterator:

```` js
// ...
// initialize

var translit = require('transliteration')

md.renderer.rules.phon = function(token, idx) {
  return translit.slugify(token[idx].content);
};
````

## License

[MIT](LICENSE)
