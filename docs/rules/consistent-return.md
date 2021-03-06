---
title: Rule consistent-return
layout: doc
---
<!-- Note: No pull requests accepted for this file. See README.md in the root directory for details. -->

# Require Consistent Returns (consistent-return)

One of the confusing aspects of JavaScript is that any function may or may not return a value at any point in time. When a function exits without any `return` statement executing, the function returns `undefined`. Similarly, calling `return` without specifying any value will cause the function to return `undefined`. Only when `return` is called with a value is there a change in the function's return value.

Unlike statically-typed languages that will catch when a function doesn't return the type of data expected, JavaScript has no such checks, meaning that it's easy to make mistakes such as this:

```js
function doSomething(condition) {

    if (condition) {
        return true;
    } else {
        return;
    }
}
```

Here, one branch of the function returns `true`, a Boolean value, while the other exits without specifying any value (and so returns `undefined`). This may be an indicator of a coding error, especially if this pattern is found in larger functions.

## Rule Details

This rule is aimed at ensuring all `return` statements either specify a value or don't specify a value.

It excludes constructors which, when invoked with the `new` operator, return the instantiated object if another object is not explicitly returned.  This rule treats a function as a constructor if its name starts with an uppercase letter.

Examples of **incorrect** code for this rule:

```js
/*eslint consistent-return: "error"*/

function doSomething(condition) {

    if (condition) {
        return true;
    } else {
        return;
    }
}

function doSomething(condition) {

    if (condition) {
        return;
    } else {
        return true;
    }
}

function doSomething(condition) {

    if (condition) {
        return true;
    }
}
```

Examples of **correct** code for this rule:

```js
/*eslint consistent-return: "error"*/

function doSomething(condition) {

    if (condition) {
        return true;
    } else {
        return false;
    }
}

function Foo() {
    if (!(this instanceof Foo)) {
        return new Foo();
    }

    this.a = 0;
}
```

## When Not To Use It

If you want to allow functions to have different `return` behavior depending on code branching, then it is safe to disable this rule.

## Version

This rule was introduced in ESLint 0.4.0.

## Resources

* [Rule source](https://github.com/eslint/eslint/tree/master/lib/rules/consistent-return.js)
* [Documentation source](https://github.com/eslint/eslint/tree/master/docs/rules/consistent-return.md)
