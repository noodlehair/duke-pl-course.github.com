subtitle: JavaScript

---

title: JavaScript Overview
build_lists: true

JavaScript is ...

* Created by [Brendan Eich][] in 1995 while working for Netscape
* A cross-platform, object-oriented, loosely typed scripting language, designed to run in a host environment (ex: browsers, Node, or other applications)
* The first [Prototype][] and [lambda][] language to go mainstream
* More closely related to [Self][] or [Scheme][] than C or Java, despite its name

[Brendan Eich]: https://brendaneich.com/
[lambda]: http://en.wikipedia.org/wiki/Lambda_(programming)
[Prototype]: http://en.wikipedia.org/wiki/Prototype-based_programming
[Self]: http://research.sun.com/self/language.html
[Scheme]: http://javascript.crockford.com/little.html

---

title: Code Example

---

title: Syntax
class: segue dark

---

title: Three Primitive Types

boolean
: `true` or `false`

number
: 64 bit floating number. No integers. Many constants are defined in the Number and Math classes, including `NaN`, `Infinity`, `E`, `PI`, etc.

string
: sequence of zero or more Unicode characters enclosed by `'` or `"`. Characters can be escaped with `\`

---

title: Boolean Values

### Logic Operators

* and: `&&`
* or `||`
* not `!`
* comparisons `>`, `<`, `>=`, `<=` 
* equals `==`, `===`
* not equals `!=`, `!==` 

```javascript
true && true   # > true
true && false  # > false
true || false   # > true
false || false  # > false


```

---

---

title: Falsy Values



---

title: Special Values

Two special values
1. `null`
2. `undefined`

---

title: Objects

Any other data type, such as `Function`, `RegExp`, `Date`, etc., are implemented as objects

An **object** is a basically a hashtable, but none of the hash-table nature (such as hash functions or rehashing methods) is visible.

**Keys** are strings, or other elements such as numbers that are converted to strings

**Values** can be any of the data types, including other objects

--- 

title: Objects Usage

`{}` is equivalent to `new Object()`. Both create a new object

Use subscript notation or dot notation to add, replace, or retrieve elements in the hashtable.

var obj = {};
obj['delet']
myHashtable["name"] = "Carl Hollywood";
There is also a dot notation which is a little more convenient.

myHashtable.city = "Anytown";

---

title: Arrays


---

title: Functions

---

title: What are functions in javascript?

A function is a set of statements and expressions enclosed within a special block.

Functions **are objects**, having a hidden, native link to `Function.prototype`.

Functions also have two hidden properties (its context and set of statements that it implements).

Functions are **first-class objects**; they can be used like any other value:
* They can be used as arguments
* They can be stored in datastructures (arrays, objects)
* They can be returned
* They can have properties and methods

They are unique in the fact that they can be invoked.

---

title: Writing a function

A function is made up of four parts:

* The keyword `function`
* An **optional** function name
* A set of parameters wrapped in parentheses
* A block of statements wrapped in curly braces

---

title: Writing a function

```javascript
foo(1, 2, 3); // Works fine because we declared function using
              //  the function literal at the bottom
bar(1, 2, 3); // => ReferenceError: bar is not defined

// The functions name is optional
// Here we are storing an anonymous function in a variable bar
var bar = function (a, b, c) {
  // ...
};

// functions can be stored
var obj = {
  baz: function () {
  },
  // you may use the optional name if recursion is needed
  fib: function fibbinator (n) {
    // ...
    return fibbinator(n-1) + fibbinator(n-2);
  }
};

function foo (a, b, c) {
  // You can nest functions
  function innerFoo() {
    // ...
  }
  return innerFoo();
}

```

---

title: Inner functions

You may write a function literal anywhere an expression may appear.

This means they may appear inside another function; these are called **inner functions**.

An inner function has access to the outer functions parameters and variables.

The collective of the outer function and inner function form a very expressive construct called a **closure**. We will cover closures in a bit.

```javascript
foo(4, 5);
function foo (a, b) {
  var x = a + b;  // a = 4, b = 5, x = 9
  function bar (c) {  // c = 9
    return a + b + c + x; // returns 18
  }
  return (14 * bar(a + b) / x) || 42;
}
```

---

title: Invoking a function

When invoking a function, there are two special parameters that are always included: `this` and `arguments`.

`this` gives the context of a functions invocation

`this` is often a common source of error because it changes based on how a function is invoked.

**invocation** as determined by a parser is a pair of parentheses following an expression that produces a function.

There are four ways to invoke a function:

* Method invocation
* Function invocation
* Constructor invocation
* Apply/call invocation

---

title: Invoking a function

```javascript
// Using method invocation
// this refers to the containing object's context
var obj = {
  value: 0,
  increment: function (inc) {
    inc = inc || 1;
    this.value += inc;
  }
};
obj.increment();    // obj.value = 1
obj.increment(2);   // obj.value = 3
obj['increment'](); // obj.value = 4

// Function invocation
// this refers to the global object (a mistake in the language design)
obj.quadruple = function () {
  var that = this;  // common idiomatic workaround for preserving context
  var helper1 = function () {
    that.value = that.value * 2;
  };
  var helper2 = function () {
    this.value = this.value * 2;
  };
  helper1();
  helper2.call(this); // Alternatively, bind this when invoking
}

// Constructor invocation
// this refers to the newly created object returned by new
var Counter = function (init) {
  this.value = init || 0;
}
Counter.prototype.increment = function (inc) {
  inc = inc || 1;
  this.value += inc;
}
var anotherObj = new Counter();
var bad = Counter();  // Don't do this, value gets attached to the global object

// Apply/call invocation
var incrementer = function (inc) {
  inc = inc || 1;
  this.value += inc;
};

var foo = { value: 5 };
incrementer.apply(foo, [1]);  // foo.value = 6
incrementer.call(foo, 2);     // foo.value = 8
```