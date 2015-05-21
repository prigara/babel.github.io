---
layout: docs
title: FAQ
description: Frequently Asked Questions and Answers
permalink: /docs/faq/
---

## What is the browser compatibility?

As a rule of thumb, IE9+. You can support IE8 by limiting yourself to a subset of ES6 features. The
[Caveats](/docs/advanced/caveats) page goes into the details of supporting legacy browsers.

## Why am I getting a syntax error when using `{ ...props }` and `async function foo() {}`?

You need to enable the [experimental option](/docs/usage/experimental).

## Why is the output of `for...of` so verbose and ugly?

This is necessary in order to comply with the spec as an iterators `return` method must be called on
errors. An alternative is to enable [loose mode](/docs/advanced/loose#abrupt-completions) but please note
that there are **many** caveats to be aware of if you enable loose mode and that you're willingly choosing
to be spec incompliant.

Please see [google/traceur-compiler#1773](https://github.com/google/traceur-compiler/issues/1773) and
[babel/babel/#838](https://github.com/babel/babel/issues/838) for more information.

## Why are `this` and `arguments` being remapped in arrow functions?

Arrow functions **are not** synonymous with normal functions. `arguments` and `this` inside arrow functions
reference their *outer function* for example:

```javascript
var user = {
  firstName: "Sebastian",
  lastName: "McKenzie",
  getFullName: () => {
    // whoops! `this` doesn't actually reference `user` here
    return this.firstName + " " + this.lastName;
  }
};
```

Please see [#842](https://github.com/babel/babel/issues/842), [#814](https://github.com/babel/babel/issues/814),
[#733](https://github.com/babel/babel/issues/733) and [#730](https://github.com/babel/babel/issues/730) for
more information.

## Why is `this` being remapped to `undefined`?

Babel assumes that all input code is an ES2015 module. ES2015 modules are implicitly strict mode so this means
that top-level `this` is not `window` in the browser nor is it `exports` in node.

If you don't want this behaviour then you have the option of disabling the `strict` transformer:

```sh
$ babel --blacklist strict script.js
```

```javascript
require("babel").transform("code", { blacklist: ["strict"] });
```

**PLEASE NOTE:** If you do this you're willingly deviating from the spec and this may cause future
interop issues.

See the [strict transformer docs](/docs/advanced/transformers/other/strict) for more info.
