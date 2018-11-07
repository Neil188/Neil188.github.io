---
layout: post
title: JavaScript's Number type and TC39 proposals
categories: javascript
tags: [javascript,console,nodejs]
---

The [tc39/proposals repository](https://github.com/tc39/proposals "link to tc-9 proposals repo"){{ site.new_tab }} tracks current stage 1 or higher proposed changes to the ECMAScript specification.  I've picked out 5 proposals would affect numerics which are worth monitoring to see how they develop.

<!--more-->

## Numeric Separator

The numeric separator [proposal](https://github.com/tc39/proposal-numeric-separator){{ site.new_tab }} is at Stage 2 (Draft), this is a proposal to add numeric literal separators to numeric literals, creating a visual separation between groups of digits.  The proposal is to use the `_` character as the separator, for instance:

```js
let million = 1_000_000;
let millionth = 0.000_001
```

This could be useful in financial fields which could help clarify between a value stored in pounds `1` or in pence `1_00`.

This proposal is on hold due to a [possible conflict](https://github.com/tc39/proposal-extended-numeric-literals/issues/7#issuecomment-379311678){{ site.new_tab }} with the [Extended Numeric Literals](#extended-numeric-literals) proposal.

You can try numeric separators today, using the Babel plugin [@babel/plugin-proposal-numeric-separator](https://babeljs.io/docs/en/babel-plugin-proposal-numeric-separator){{ site.new_tab }}

Further reading: [2ality - ES proposal: numeric separators](http://2ality.com/2018/02/numeric-separators.html){{ site.new_tab }}

## Extended Numeric Literals

The Extended Numeric Literals [proposal](https://github.com/tc39/proposal-extended-numeric-literals){{ site.new_tab }} is at Stage 1 (Proposal), this is a proposal to allow type identifiers as a suffix to a numeric literal, in the format *numericLiteral_identifier*.

The example given in the proposal is for a CSS unit, in the format `style.fontSize = 3_px`.

This could help if new numeric types are added to the language such as [Decimals](#decimals).

This proposal has a [possible conflict](https://github.com/tc39/proposal-extended-numeric-literals/issues/7#issuecomment-379311678){{ site.new_tab }} with the [Numeric Separator](#numeric-separator) proposal.

## Math.signbit

[`Math.signbit`: IEEE-754 sign bit](http://jfbastien.github.io/papers/Math.signbit.html){{ site.new_tab }} is a stage 1 proposal to add a signbit method to the Math object.
The IEEE 754 standard that JavaScript uses for its numeric types allows for `-0` and `+0` values.  To check the sign of a 0 value you can test either the result of dividing by zero, or by using `Object.is(-0, variable)`:

```js
const x = 0
const y = -0
1 / x               // Infinity
1 / y               // -Infinity
Object.is(-0, x)    // false
Object.is(-0, y)    // true
```

The current `Math.sign` method will return `0` or `-0` when used on positive or negative zero values:

```js
Math.sign(0);       //  0
Math.sign(-0);      // -0
```

But as JavaScript considers `0` and `-0` equal this doesn't give you a simple test for `-0`.

This proposal will add a `Math.signbit(variable)` method, which will return `true` for all negative values, including `-0`.

## Decimals

The decimals proposal is a stage 0 (Strawman), so is not an active proposal (see the [repo](https://github.com/tc39/proposals/blob/master/stage-0-proposals.md "Stage 0 proposals document"){{ site.new_tab }} for a list of stage 0 proposals, with the last presented date).

The goal of this proposal is to have guaranteed decimal precision in the language.  JavaScript uses the IEEE 754 format (see [JavaScript Numbers post]({{  site.baseurl  }}{%  post_url  2018-10-27-JavaScript-Numbers  %})) which has a problem with the precision of floating point values:

```js
0.1 + 0.2           // 0.30000000000000004
```

This proposal is an investigation into the different options for resolving this issue.  The proposal suggests three different new numeric types - Rationals, Fixed-size decimal and BigDecimal as possible solutions.

This is a stage 0 proposal and may wait a long time before it gets an update.  If you do need precision integers in your code, for instance for a finance application, a possible solution could be the [bigdecimal.js](https://github.com/iriscouch/bigdecimal.js){{ site.new_tab }} library, or [decimal.js](https://github.com/MikeMcl/decimal.js){{ site.new_tab }}.

## BigInt

The bigint [proposal](https://github.com/tc39/proposal-bigint){{ site.new_tab }}, at stage 3, is actually a new primitive type separate from the Number primitive.  BigInts provide a way to represent whole numbers larger than 2^53^, which is the largest number JavaScript can reliably represent with the `Number` primitive.

Adding a BigInt type will also allow the addition of a possible BigDecimal type at some point.

Create a `BigInt` by appending `n` to the end of the integer or by calling the `BigInt` constructor.  Note the value created will have a type `bigint` and this will not be compatible with `number` types:

```js
var x = 123         // undefined
var y = 456n        // undefined
typeof x            // "number"
typeof y            // "bigint"
x + y               // VM726:1 Uncaught TypeError:
                    // Cannot mix BigInt and other types,
                    // use explicit conversions
```

Note that bigint types are **not** meant as a generic integer type, so are only recommended for large numbers, greater than 2^53^.  For this reason, its unlikely that it will be (correctly) used often.

A Babel [plugin](https://babeljs.io/docs/en/babel-plugin-syntax-bigint){{ site.new_tab }} is available to try bigints.

V8 version 6.7 included BigInts and are in work for SpiderMonkey (Firefox) and JSC (Edge).

If you have the latest Chrome installed you should already be able to try testing BigInt types, Chrome 67 Stable included V8 version 6.7, you can check the version of V8 Chrome is using by going to chrome://version/, which will list the V8 version under the JavaScript section.

The current node LTS (at the time of writing LTS is 10.13.0 ) has V8 version 6.8 so also includes BigInt types.  You can check which version of v8 your installed version of node is using with command `node -p process.versions.v8`.  To see which versions of node have BigInts check [node.green](https://node.green/#ESNEXT-candidate--stage-3--BigInt){{ site.new_tab }}

Further reading on BigInts:

* [Google Developers post](https://developers.google.com/web/updates/2018/05/bigint){{ site.new_tab }}
* [V8 Blog post](https://v8.dev/blog/bigint){{ site.new_tab }}
* [2ality post](http://2ality.com/2017/03/es-integer.html){{ site.new_tab }}
