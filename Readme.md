<div                                                         id="/">&nbsp;</div>
[![Coveralls – test coverage
](https://img.shields.io/coveralls/studio-b12/tape-css.svg?style=flat-square)
](https://coveralls.io/r/studio-b12/tape-css)
 [![Travis – build status
](https://img.shields.io/travis/studio-b12/tape-css.svg?style=flat-square)
](https://travis-ci.org/studio-b12/tape-css)
 [![David – status of dependencies
](https://img.shields.io/david/studio-b12/tape-css.svg?style=flat-square)
](https://david-dm.org/studio-b12/tape-css)
 [![Stability: alpha
](https://img.shields.io/badge/stability-alpha-yellowgreen.svg?style=flat-square)
](https://github.com/studio-b12/tape-css/issues/3)
 [![Code style: airbnb
](https://img.shields.io/badge/code%20style-airbnb-777777.svg?style=flat-square)
](https://github.com/airbnb/javascript)




<p                                                                   >&nbsp;</p>

tape-css
========

**CSS unit testing. Ligtening-fast. tape-style.**

Isolates DOM and styles for ligtening-fast unit testing. As elegant and lightweight as *[tape][]* itself.

[tape]:  https://www.npmjs.com/package/tape




<p align="center">
  <br/>
  <img
    alt="Screencast"
    src="Readme/screencast.gif"
    width="640"
    height="464"
  />
  <div align="right"><sup>
    We use <em><a href="https://www.npmjs.com/package/hihat">hihat</a></em> and <em><a href="https://www.npmjs.com/package/tap-dev-tool">tap-dev-tool</a></em> together with <em>tape-css</em> in the screencast.
  </sup></div>
  <br/>
</p>




<a                                                 id="/installation"></a>&nbsp;

Installation
------------

```sh
$ npm install tape-css
```




<a                                                        id="/usage"></a>&nbsp;

Usage
-----

######  1

Pick your favorite flavor of *[tape][]* – be it *[tape][]* itself, *[tape-catch][]*, *[blue-tape][]* or whatever.

```js
const tape = require('tape');
const test = require('tape-css')(tape);  // We don’t change `tape` in any way.
```

[tape-catch]:  https://www.npmjs.com/package/tape-catch
[blue-tape]:   https://www.npmjs.com/package/blue-tape

######  2

Pass the DOM tree and styles you want to test. We’ll add it to the `<body>`[\*](https://github.com/studio-b12/tape-css/issues/1) before your test begins – and clean them up right after it has ended.

```js
test('Roses are red, <span>s are blue', {
  dom: document.createElement('span'),
  styles: '* { color: red; } span { color: blue; }',
}, (t) => {
  // Our span and styles are here to play with.
  t.equal(
    getComputedStyle(document.querySelector('span')).getPropertyValue('color'),
    'rgb(0, 0, 255)'
  );

  t.end();
  // We’ve now cleaned up the place!
});
```

######  3

*tape-css* is made to play well with other tools. *[hyperscript][]* and *[computed-style][]* can make your tests nicer to read and write:

```js
const h = require('hyperscript');
const style = require('computed-style');
const personOne = h('.person');
const personTwo = h('.person');

test('Everyone has some space to breathe', {
  dom: h('div', [personOne, personTwo]),
  styles: '.person { margin-bottom: 10px } * { padding: 5px }',
}, (is) => {
  is.equal(
    personTwo.getBoundingClientRect().bottom -
    personOne.getBoundingClientRect().top,
    10,
    '10 px between people'
  );

  is.equal(
    style(personOne, 'padding-right'),
    '5px',
    'one has room to move his arm'
  );

  is.end();
})
```

[hyperscript]:      https://www.npmjs.com/package/hyperscript
[computed-style]:   https://www.npmjs.com/package/computed-style

######  4 (work in progress!)

Whenever you want to see how your layout actually looks like, use `test.only`. We’ll only execute this one test and we won’t reset the DOM and styles afterwards. That’s pretty useful while debugging.

```js
test('All is well', /* … */);
test.only('Need to debug this', /* … */);
test('Works alright', /* … */);
```

NOTE: This probably doesn’t work yet. Comment out your `t.end()` to get a similar effect.




<a                                                          id="/api"></a>&nbsp;

API
---

<!-- @doxie.inject start -->
<!-- Don’t remove or change the comment above – that can break automatic updates. -->
<div align="right"><sub>JSIG SIGNATURE <a href="http://jsig.biz/">(?)</a></sub></div>
```js
test(tape) => (
  name?        : String,
  options?: {
    // All original `tape` options, and:
    dom?       : Element | DocumentFragment
    styles?    : String
    document?  : Document
  },
  callback     : Function
) => void
```

If you use tape, you’ll feel right at home. Give us an instance of `tape`.
We won’t change its [existing API][] in any way. We just add a couple
of `options`:

- `dom` – one or more DOM elements. We’ll add it to the `<body>`
  before your test and clean it up after your test has ended.
  Default: nothing.

- `styles` – a string of CSS. We’ll add it as a `<style>` to the `<head>`
  before your test – and clean it up after your test has ended.
  Default: nothing.

- `document` – a custom implementation of `document`. It may be useful
  for testing outside a browser. Default: `window.document`.

[existing API]:     https://github.com/substack/tape#methods
<!-- Don’t remove or change the comment below – that can break automatic updates. More info at <http://npm.im/doxie.inject>. -->
<!-- @doxie.inject end -->




<a                                                      id="/credits"></a>&nbsp;

Credits
-------

This module was inspired by *[quixote](https://github.com/jamesshore/quixote)*. Thumbs up for the great idea [@jamesshore](https://github.com/jamesshore)!

It turned out that *quixote* wasn’t easy to use with *tape* though. As well as that, it comes with heavy abstractions (over 3K lines of code) and its own assertion engine – while everything you need for assertions comes with *tape* already.

We were after something simple which does one thing (<50 LOC last time we checked). And plays well with other simple tools.




<a                                                      id="/license"></a>&nbsp;

License
-------

[MIT][] © [Studio B12 GmbH][]

[MIT]:              ./License.md
[Studio B12 GmbH]:  http://studio-b12.de
