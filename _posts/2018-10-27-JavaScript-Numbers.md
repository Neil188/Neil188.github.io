---
layout: post
title: JavaScript Number type
categories: javascript
tags: [javascript,console,nodejs]
---

Number is one of the six primitive data types in JavaScript, unlike in other languages where numbers can be represented in multiple types such as integers, float, double etc. JavaScript only has the one numeric data type (at the moment).

<!--more-->

The Number type is defined in the [TC39 spec](https://tc39.github.io/ecma262/#sec-ecmascript-language-types-number-type "TC39 Number Type"){{ site.new_tab }} as in the "double-precision 64-bit format IEEE 754-2008 values as specified in the IEEE Standard for Binary Floating-Point Arithmetic".  Understanding how numbers are stored internally, or learning this standard isn't necessary, but it is important to understand the implications it can have on your programs.

## Calculating floating point values

Probably the most common problem and source of errors is a result of storing numbers using this number standard (note this is a problem of the number standard, not a problem specific to JavaScript).

```javascript
>  0.1 + 0.2
// 0.30000000000000004
```

Always be aware of possible precision problems when working with floating point values, if precision is required then  use a function similar to the following to add the values as integers:

```javascript
function addValues(...vals) {
    function decimalPlaces(x) {
        return x.toString().replace(/\d*\.?/,'').length
    }
    const mult = vals.reduce( (a,b) => {
        const dec = decimalPlaces(b);
        return dec > a ? dec : a
    }, 0)
    return vals.reduce( (a,b) => a + b * 10 ** mult, 0 ) / 10 ** mult;
}
```

## Max integer values and safe integers

Integer values can also have problems once you reach large numbers, though this isn't something you should encounter very often.  The Number.MAX_VALUE constant represents the maximum numeric value representable in JavaScript:

```javascript
>  Number.MAX_VALUE
// 1.7976931348623157e+308
```

Values larger than this number are stored as infinity.

JavaScript also has a Number.MAX_SAFE_INTEGER constant, which is the largest integer n such that n *and* n + 1 are both exactly stored.  Note this number is not the same as MAX_VALUE - larger values than MAX_SAFE_INTEGER can be stored, but the value may not be correct.

```javascript
>  Number.MAX_SAFE_INTEGER
// 9007199254740991
>  Number.MAX_SAFE_INTEGER +1
// 9007199254740992
>  Number.MAX_SAFE_INTEGER +2
// 9007199254740992
```

The above shows that `Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2` which isn't correct.

This has implications when performing calculations and when parsing JSON data, which can be corrupted as a result of the coercion to the Number type:

```javascript
>  JSON.parse( '{"test":9007199254740993}' )
// {test: 9007199254740992}
```

## Zero

JavaScript has both a positive zero (`+0` or just `0`) and a negative zero (`-0`). Again this isn't a feature of JavaScript, but comes from the IEEE 754 standard for floating-point arithmetic - ["The IEEE 754 standard for floating-point arithmetic (presently used by most computers and programming languages that support floating point numbers) requires both +0 and −0."](https://en.wikipedia.org/wiki/Signed_zero "Wikipedia Signed zero"){{ site.new_tab }}.  For practical purposes, this will have no impact on most people writing JavaScript code, as [ECMAscript spec](http://www.ecma-international.org/ecma-262/#sec-strict-equality-comparison "ECMA Strict Equality Comparison") explicitly says :

> d. If x is +0 and y is -0, return true.  
> e. If x is -0 and y is +0, return true.

So `-0` will always equal `+0`

``` javascript
>  -0 === +0
//  true
```

And in fact, trying to test for `-0` is problematic due to this equality rule

``` javascript
>  123 * -0
//  -0

>  123 * -0 === 0
// true

>  var test = 123 * -0
// undefined
>  test === 0
// true
>  test.toString()
// "0"

>  -0 < +0
// false
```

But if you do need to test for `-0` you can use the following:

```javascript
>  1/0
// Infinity
>  1/-0
// -Infinity
```

as `1` divided by `-0` gives you negative Infinity, you can use this to test for a negative zero value:

```javascript
const isNegative = x => (1/x) < 0

isNegative(-0) // true
isNegative(0) // false
```

## Infinity

The Number object has properties for `Number.POSITIVE_INFINITY` and `Number.NEGATIVE_INFINITY`.  The global object also has an `Infinity` property, which is set to `Number.POSITIVE_INFINITY`.

As well as checking for equality using `x === Infinity` we also have access to `isFinite()` methods for checking if a value is a finite number.  There are actually two available versions of this - the global `isFinite`, and `Number.isFinite()`, the difference being that the global version will coerce any values passed to it to a number, whereas the Number method will not convert the type before checking, when using the method always be aware of which you are using as they will give different results:

```javascript
>  isFinite('1')
// true
>  Number.isFinite('1')
// false
```

## NaN

Another special number value is NaN, or Not a Number, available as a property of both global and Number.  A feature of the NaN value is that it will always compare unequal to any other value - including to another NaN value

```javascript
> NaN === NaN
// false
> NaN !== NaN
// true
```

As a result of you can check for NaN by performing a self-comparison (as all other values will return true when compared to themselves):

```javascript
function myIsNaN(x) { return x !== x }
```

However the `isNaN()` function is available to do this for you.  Note that, like `isFinite()`, the `isNaN()` function is available as a global function and a method on Number.  Again the difference is that the global version will always coerce the value passed to it to a Number type first, so you may get unexpected results:

```javascript
> isNaN('test')
// true
> Number.isNaN('test')
// false
```

If the second value doesn't look correct to you, remember that the purpose of the function is not to test whether the value is numeric, but to test whether is specifically set to the NaN value.

## More Reading

[MDN Docs on the Number object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number "MDN Number Object"){{ site.new_tab }}

[An excellent article on how numbers are technically represented in JavaScript by  Max Koretskyi](https://medium.com/dailyjs/javascripts-number-type-8d59199db1b6 "Medium article Here is what you need to know about JavaScript’s Number type"){{ site.new_tab }}

[ECMAScript spec - number object](https://www.ecma-international.org/ecma-262/#sec-number-objects){{ site.new_tab }}
