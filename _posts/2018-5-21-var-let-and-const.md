---
layout: post
title: var, let and const
categories: javascript
tags: [javascript,scope]
---

Introduced in ES2015 `let` and `const` are additional ways to declare variables, alongside the traditional `var` keyword.

<!--more-->

When variables are created with `var` they are either function scoped, when created inside of a function, or globally scoped if created outside of a function, at which point they are available as global variables.

## Hoisting

All variable `var` declarations are processed before any code is executed, in a behaviour that is known as 'hoisting', so the variable declaration behaves as if moved to the top of the function, or the global code.  Note that this hoisting only applies to the declaration, not the initialisation - the variable can be used before declaration/initialisation step occurs, but it will be populated as `undefined`.  If you redeclare a `var` variable, it will not lose its value, ie:

```javascript
var test = 1;
test = 2;
var test;
test; // 2
```

As effectively both `var` declarations are hoisted to the top of the function or global scope.

Because of this hoisting, it is always best practice to declare variables at the top of their scope

## Accidental globals

A common problem with `var` declared variables is that they can leak into the global scope, causing unexpected problems.  Mistakingly assuming a variable is local when declared within an `if` block for instance:

```javascript
if (true) {
   var test = 'oops, a global variable'
}
console.log(test); // oops, a global variable
```

Or when using loops:

```javascript
for (var x = 1; x < 10 ; x++) {
   console.log(x);
}
```

In this case, `x` is now on the global scope, if you then have another section of code using `x` this could cause unexpected problems when the variable has already been changed by your `for` loop.

## For loops and re-using variables

Another common problem is reusing a variable in a `for` loop:

```javascript
for (var x = 0; x < 10 ; x++) {

   setTimeout(() => console.log(x), 0)

}
```

This will output '10' ten times - because the `x` variable is only getting created once, so each `setTimeout` callback is pointing to the same `x` variable, which will be set to 10 at the time the `for` loop exits.

## let and const

The `let` and `const` keywords introduced in ES2015, give us variable declarations with block scope, so they will not leak from a block of code into the global scope.  In addition, both let and const declarations at the top level of the code do not create a property on the global object:

```javascript
var test1 = 'test'

let test2 = 'test2'

window.test1
 // "test"

window.test2 //
undefined
```

### let

The `let` keyword allows you to declare a block scoped local variable, which can be re-assigned (but not redeclared).  When used in a for loop, the let variable will actually rebind the variable for each iteration of the loop, so the above for...setTimeout code can be changed to a `let` variable:

```javascript
for (let x = 0; x < 10 ; x++) {

   setTimeout(() => console.log(x), 0)

}
```

And now this will output the count from 0 to 10.

### const

The `const` keyword allows you to declare a block scoped local variable, which cannot be re-assigned or redeclared.  Its value must be assigned at the declaration point.

A key point to stress here is that a `const` variable creates a read-only *reference* to a value, it is not a constant value:

```javascript
const test = [];
test.push('x'); // adds x to array, no errors
test = []; // Uncaught TypeError: Assignment to constant variable.
```

### Temporal Deadzone and Hoisting

Unlike `var`, both `const` and `let` cannot be used before they are declared, however, they do have some hoisting behaviour.  The period between the start of the current scope, and the let/const variable being declared is known as the Temporal Deadzone, a good description of this can be found at [JS Rocks](http://jsrocks.org/2015/01/temporal-dead-zone-tdz-demystified){{ site.new_tab }} which has the following example:

```javascript
let x = 'outer scope';
(function() {
    console.log(x);
    let x = 'inner scope';
}());
```

This throws an `Uncaught ReferenceError: x is not defined` error, as the inner `x` has been hoisted, but is now being referenced before it is declared, causing the error.

This behaviour is actually useful for finding bugs, as it forces you to think about where you are declaring and using your variables.

### Which to use

I would recommend always using `const` where a variable doesn't need to be reassigned, otherwise use `let`.  However, when working in a REPL (Read, Eval, Print Loop) environment for testing, it's probably better to use `var` as it lets you redeclare variables.
