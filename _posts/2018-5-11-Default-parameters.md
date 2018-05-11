---
layout: post
title: Default parameters
categories: javascript
tags: [javascript]
---

Default parameters allow you to initialise parameters with default values and are an incredibly useful tool in JavaScript.

<!--more-->

Any function parameters will default to `undefined`, so if they aren't passed as arguments a common strategy is to test the parameter in the function body and set a default if not provided:

```javascript
function myAddFunc (firstParam, missingParam) {
   missingParam = (typeof missingParam !== 'undefined') ? missingParam : 10;
   return firstParam + missingParam;
}

myAddFunc(1, 2); // 3
myAddFunc(1); // 11
```

Or, the lazy incorrect way:

```javascript
function myAddFunc (firstParam, missingParam) {
   missingParam = !missingParam ? 10 : missingParam;
   return firstParam + missingParam;
}

myAddFunc(1, 0); // 11 oops
```

Because of course 0 is evaluated as a falsey value so triggers the default parameter.

## ECMAScript 2015 - Default Parameters

Introduced in ECMAScript 2015, default parameters allow anything omitted from a function call, or passed as undefined to trigger the default replacement (note this does not include `NULL` - this is considered a valid value, not a missing value).

```javascript
function myAddFunc (firstParam = 0, missingParam = 10) {
   return firstParam + missingParam;
}

myAddFunc(1, 2); // 3
myAddFunc(1); // 11
myaddFunc(); // 10
```

As you can see this allows you to shorten the function, and this example could be even further reduced using an arrow function:

```javascript
const myAddFunc = (firstParam = 0, missingParam = 10) => firstParam + missingParam;
```

You can also refer to previous parameters:

```javascript
function myAddFunc (firstParam = 0, missingParam = firstParam + 10) {
   return firstParam + missingParam;
}

myAddFunc(1, 2); // 3
myAddFunc(1); // 12
myaddFunc(); // 10
```

Note you only refer to previous parameters that have already been encountered, so firstParam could not reference missingParam like this:

```javascript
function myAddFunc (firstParam = missingParam, missingParam = 10) {
   return firstParam + missingParam;
}

myAddFunc() // Uncaught ReferenceError: missingParam is not defined
```

You can even use a function as the default value, for instance, to calculate the value or return an error:

```javascript
const requiredParam = (param) => {
   const requiredParamError = new Error(`Required param ${param} is missing`)
   throw requiredParamError
}

const myFunc = ({
   firstParam,
   secondParam=requiredParam('secondParam')
} = {}) => console.log(firstParam, secondParam)

myFunc({firstParam: '1', secondParam: '2'}) // 1 2
myFunc() // Uncaught Error: Required param secondParam is missing
```

Note the multiple levels of defaults for the parameters - { ... } = {} defaults to an object if no parameters are provided.  The secondParam=requiredParam('secondParam') is then also applied, so it hasn't just defaulted to an empty object and stopped at that point.

## Object deconstructuring

You can also use default value with object deconstructuring:

```javascript
const test = { a: 1 };
const { a: var1, var2 = 2 } = test;
var1 // 1
var2 // 2
```

## Added bonus - parameter hints

In VS Code (other editors will provide similar features) IntelliSense will give you parameter hints, and if you have used default values this will include a hint to the type of parameter that is expected:

![image text]({{ "/images/posts/Default parameters - vs code hints.png" | absolute_url }})