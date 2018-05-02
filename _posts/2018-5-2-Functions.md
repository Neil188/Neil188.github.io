---
layout: post
title: Functions
categories: javascript
tags: javascript
---

Functions are a key part of JavaScript, so getting a good understanding of them is essential.

<!--more-->

## Types of functions

### Function declaration, or function statement

```JavaScript
function funcDeclaration (arg1) {
   return true;
}
```

Function declarations in JavaScript are hoisted to the top of the enclosing function or global scope. You can use the function before you declared it

### Function expressions

Function expressions can be named or anonymous functions:

```JavaScript
const myFunc = function funcExpression (arg1) {
   return true;
}
const myAnonFunc = function (arg1) {
   return true;
}
```

Unlike function declarations, function expressions are not hoisted.  Note if you tried changing `const myFunc` to `var myFunc` this does not hoist the function expression, only the declaration of the myFunc variable (which would be set as `var myFunc = undefined`).

#### Anonymous functions and debugging

Using anonymous functions does reduce the need to name everything (and naming things can get very difficult!) however when it comes to debugging your application, they can be frustrating.  Whenever you view a stack trace (in the DevTools debugger, in the output of an error message, or via the console.trace command) you will see the not very useful anonymous function:

```javascript
setTimeout( function test() { console.trace('named') }, 0)
setTimeout( function () { console.trace('anon') }, 0)
> named
   test
> anon
   (anonymous)
```

which isn't going to help you very much!

## [IIFE](https://en.wikipedia.org/wiki/Immediately-invoked_function_expression){{ site.new_tab }} - Immediately Invokable Function Expression

IIFE are function expressions that are invoked as soon as the function is declared.

```javascript
(function () {
   console.log('IIFE')
})()
// alternative format
(function () {
   console.log('IIFE')
}())
```

The outer ( ... ) prevents the function from being processed as a normal function declaration, the `()` executes the function declared before it. If you try the same without the enclosing ( ... ) you get an error

```javascript
function () {
   console.log('IIFE')
}()
Uncaught SyntaxError: Unexpected token (
```

As this would be treated as a function declaration, and as such expects a name for the function, so the syntax is invalid.
An IIFE can be named however and it can be argued that naming your function is useful as it provides a more useful stack trace, even if you just name it IIFE:

```javascript
(function IIFE() {
   console.log('IIFE')
})()
```

But again if you omit the enclosing ( ... ) you get an error, but this time a different error:

```javascript
function IIFE() {
   console.log('IIFE')
}()
Uncaught SyntaxError: Unexpected token )
```

This time the error is complaining about the closing ), this is because this is now a statement, and the parse is now expecting a separate grouping statement.  If you put something within the brackets you can see it being evaluated, but your function doesn't run:

```javascript
function IIFE() {
   console.log('IIFE')
}(console.log('hello'))
> hello
```

The parser is now seeing a function statement, followed by another statement, so you won't get the IIFE you desire.  Which is a very long way of saying put brackets around your function if you want an IIFE, or it won't work!

A common reason for using IIFE is to protect against polluting the global environment and prevent namespace collision with any other libraries that you use.  As JavaScript has function scoping, anything declared within the IIFE (and the IIFE function itself if declared as a named function) are not available outside of the IIFE

## [Arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions){{ site.new_tab }}

Introduced in ES6m an arrow function expression has a shorter syntax compared to function expressions, omitting the function keyword:

```javascript
const myFunc = (arg1) => {
   return arg1
}
```

you can also use an even shorter concise body syntax:

```javascript
const myFunc = (arg1) => arg1
```

Where the expression becomes the implied return value, in this case, arg1 is the return value.  In addition, if only a single argument is passed in the parenthesis are optional:

```javascript
const myFunc = arg1 => arg1
```

One thing to note is that when passing in or returning an object literal, you will need to wrap the object in parenthesis:

```javascript
const myFunc = () => ({ a : 1 })
```

### Differences in the behavior of arrow functions

* Arrow functions are always anonymous.
* Arrow functions do not have their own this value. The value of this inside an arrow function is always inherited from the enclosing scope.
* Arrow functions do not have the 'arguments' object

    ```javascript
    > function args( arg1, arg2, arg3) {
        console.log(arguments);
    }

    > args(1,2,3)
    > Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]

    > var args2 = ( arg1, arg2, arg3) => {
        console.log(arguments);
    }

    > args2(1,2,3)
    > Uncaught ReferenceError: arguments is not defined
        at args2 (<anonymous>:2:14)
        at <anonymous>:1:1
    ```

* Arrow functions do not have a prototype property.

    ```javascript
    >args.prototype
    >{constructor: ƒ}constructor: ƒ args( arg1, arg2, arg3)__proto__: Object
    >args2.prototype
    >undefined
    ```

* The yield keyword may not be used in an arrow function's body (except when permitted within functions further nested within it). As a consequence, arrow functions cannot be used as generators.

### IIFEs

Arrow functions can also be used as the IIFE body:

```javascript
( () => console.log('hello') ) ()
hello
```
