# PHASE 2.5 — PROTOTYPE & PROTOTYPE CHAIN (Deep Dive)

This is one of the MOST IMPORTANT concepts in JavaScript.

If closures explain memory behavior…

then prototypes explain:

```text id="yzxyur"
Inheritance in JavaScript
```

Without understanding prototypes:

* classes feel magical
* inheritance feels confusing
* method sharing unclear
* `new` keyword unclear
* `this` confusing
* object lookup mysterious

Because JavaScript is NOT class-based internally.

It is:

# Prototype-Based Language

This is HUGE.

---

# FIRST BIG IDEA

Most languages:

```text id="9i3j4g"
Class → creates objects
```

JavaScript originally had NO classes.

Objects inherit directly from other objects.

Using:

# Prototype Delegation

Classes came later as syntax sugar.

---

# SIMPLE DEFINITION

Prototype is:

```text id="eq5r95"
An object from which another object inherits properties/methods
```

---

# EVERY OBJECT IN JS HAS HIDDEN LINK

Every object contains hidden internal property:

```text id="hr24qk"
[[Prototype]]
```

which points to another object.

---

# VISUAL

```text id="1kktwa"
obj
 ↓
[[Prototype]]
 ↓
another object
```

This chain enables inheritance.

---

# MOST IMPORTANT RULE

When property not found in object:

```text id="4v7pcp"
JavaScript searches prototype chain upward
```

This is EVERYTHING.

---

# SIMPLE EXAMPLE

```js id="yc34jr"
const user = {
  name: "Musharaf"
};

console.log(user.toString());
```

WAIT…

We never added:

```js id="mgit5u"
toString()
```

Then HOW does it exist?

Answer:

# Prototype Chain

---

# INTERNAL SEARCH

JS searches:

---

# STEP 1

Inside `user`.

```text id="1n0n5k"
name → found
toString → not found
```

---

# STEP 2

Follow hidden prototype link.

```text id="yrp75y"
user.[[Prototype]]
```

points to:

```text id="l9m0xh"
Object.prototype
```

There:

```js id="q1oj7e"
toString()
```

exists.

Thus function found.

---

# THIS IS PROTOTYPE DELEGATION

Object delegates lookup to prototype.

---

# IMPORTANT VISUAL

```text id="d29q2u"
user object
   ↓
Object.prototype
   ↓
null
```

This linked structure:

# Prototype Chain

---

# HOW TO ACCESS PROTOTYPE

---

# OLD WAY

```js id="2ikdjr"
user.__proto__
```

Not recommended officially.

---

# MODERN WAY

```js id="e75epp"
Object.getPrototypeOf(user)
```

---

# TRY THIS

```js id="s8fwow"
const user = {};

console.log(Object.getPrototypeOf(user));
```

Output:

```text id="quefb6"
Object.prototype
```

---

# IMPORTANT

Almost every normal object eventually inherits from:

```text id="e3rb0m"
Object.prototype
```

---

# WHY ARRAYS HAVE METHODS

---

# Example

```js id="tvgbw0"
const arr = [1,2,3];

arr.push(4);
```

How does array know `push()`?

Prototype chain again.

---

# INTERNAL CHAIN

```text id="kz8r44"
arr
 ↓
Array.prototype
 ↓
Object.prototype
 ↓
null
```

---

# `push()` EXISTS HERE

```text id="1g1p50"
Array.prototype
```

---

# HUGE INSIGHT

Methods are NOT copied into every object.

That would waste memory.

Instead:

```text id="ukx37l"
Objects share methods through prototypes
```

Massive optimization.

---

# WHY PROTOTYPES EXIST

Imagine:

```js id="x8rw4u"
const user1 = {
  name: "A",

  greet() {
    console.log("hello");
  }
};

const user2 = {
  name: "B",

  greet() {
    console.log("hello");
  }
};
```

Each object stores separate `greet` function.

Wasteful.

---

# BETTER APPROACH

Store ONE shared function in prototype.

All objects reuse it.

Huge memory efficiency.

---

# CONSTRUCTOR FUNCTIONS

Before classes, JS used constructor functions.

---

# Example

```js id="jlwm3w"
function User(name) {
  this.name = name;
}

const u1 = new User("A");
const u2 = new User("B");
```

---

# WHAT DOES `new` DO INTERNALLY?

VERY IMPORTANT.

---

# STEP 1

Creates empty object.

```text id="jlwm4p"
{}
```

---

# STEP 2

Links object prototype.

```text id="jlwm5k"
newObj.[[Prototype]]
    ↓
User.prototype
```

---

# STEP 3

Binds:

```text id="jlwm6g"
this → newObj
```

---

# STEP 4

Executes constructor.

```js id="jlwm7d"
this.name = name
```

---

# STEP 5

Returns object.

---

# FINAL STRUCTURE

```text id="jlwm8a"
u1
 ↓
User.prototype
 ↓
Object.prototype
 ↓
null
```

---

# ADDING SHARED METHODS

---

# WRONG WAY

```js id="jlwm94"
function User(name) {
  this.name = name;

  this.greet = function() {
    console.log("Hello");
  };
}
```

Problem:

Every object gets NEW function copy.

Bad memory usage.

---

# CORRECT WAY

```js id="jlwm9z"
function User(name) {
  this.name = name;
}

User.prototype.greet = function() {
  console.log("Hello");
};
```

Now ALL users share ONE function.

---

# VISUAL

```text id="jlwmaw"
u1 ----\
        → User.prototype.greet
u2 ----/
```

Shared memory.

---

# HUGE INTERVIEW INSIGHT

This is WHY prototype inheritance is memory efficient.

Methods shared instead of duplicated.

---

# PROPERTY LOOKUP PROCESS

CRITICAL.

---

# Example

```js id="jlwmbu"
function User(name) {
  this.name = name;
}

User.prototype.role = "user";

const u1 = new User("A");

console.log(u1.role);
```

Output:

```text id="jlwmcq"
user
```

---

# SEARCH PROCESS

JS checks:

---

# STEP 1

Inside u1 object.

```text id="jlwmdn"
role?
```

Not found.

---

# STEP 2

Follow prototype link.

```text id="jlwmen"
u1.[[Prototype]]
```

which is:

```text id="jlwmfk"
User.prototype
```

There:

```text id="jlwmgh"
role → "user"
```

Found.

---

# IMPORTANT

Objects DO NOT copy prototype properties.

They ACCESS through delegation.

---

# PROPERTY SHADOWING

---

# Example

```js id="jlwmhe"
User.prototype.role = "user";

const u1 = new User("A");

u1.role = "admin";

console.log(u1.role);
```

Output:

```text id="jlwmib"
admin
```

---

# WHY?

Own property found first.

Search stops immediately.

Prototype property shadowed.

---

# VISUAL

```text id="jlwmj9"
u1.role → "admin"

User.prototype.role → "user"
```

Local property wins.

---

# EVERYTHING IN JS IS CONNECTED TO PROTOTYPES

Arrays:

```text id="jlwmk5"
Array.prototype
```

Functions:

```text id="jlwml2"
Function.prototype
```

Objects:

```text id="jlwmm0"
Object.prototype
```

Even functions are objects.

Huge insight.

---

# FUNCTIONS HAVE PROTOTYPE TOO

---

# Example

```js id="jlwmmy"
function test() {}

console.log(test.prototype);
```

Functions automatically get prototype object.

Used when creating instances with `new`.

---

# IMPORTANT DIFFERENCE

Very confusing topic.

---

# `__proto__`

Hidden prototype link of OBJECT.

---

# `.prototype`

Property on CONSTRUCTOR FUNCTION.

Used for future instances.

---

# VISUAL

```text id="jlwmnu"
function User() {}

User.prototype
      ↓
used by new objects

u1.__proto__
      ↓
points to User.prototype
```

This is critical.

---

# CLASS SYNTAX IS JUST SUGAR

---

# Example

```js id="jlwmoo"
class User {
  constructor(name) {
    this.name = name;
  }

  greet() {
    console.log("Hello");
  }
}
```

Internally roughly becomes:

```js id="jlwmpm"
function User(name) {
  this.name = name;
}

User.prototype.greet = function() {
  console.log("Hello");
};
```

Classes hide prototype complexity.

But internally still prototype system.

---

# INHERITANCE VIA PROTOTYPES

---

# Example

```js id="jlwmqj"
const animal = {
  eats: true
};

const dog = Object.create(animal);

console.log(dog.eats);
```

Output:

```text id="jlwmrh"
true
```

---

# WHY?

Prototype chain:

```text id="jlwmsf"
dog
 ↓
animal
 ↓
Object.prototype
 ↓
null
```

dog inherits from animal.

---

# Object.create()

Creates object with chosen prototype.

Very important.

---

# PROTOTYPE CHAIN TERMINATION

Eventually chain ends at:

```text id="jlwmtc"
null
```

---

# VISUAL FULL CHAIN

```text id="jlwmu9"
arr
 ↓
Array.prototype
 ↓
Object.prototype
 ↓
null
```

When null reached:

```text id="jlwmv5"
Search stops
```

---

# HUGE PERFORMANCE INSIGHT

Prototype lookup is dynamic.

Long chains increase lookup work slightly.

Modern engines optimize heavily though.

---

# WHY JS CHOSE PROTOTYPES

Prototype delegation provides:

* flexibility
* dynamic inheritance
* memory efficiency
* runtime modification

Very different from Java/C++ style.

---

# ADVANCED INSIGHT — METHODS ARE SHARED

This means:

```js id="jlwmw1"
u1.greet === u2.greet
```

TRUE.

Because same prototype function shared.

Huge interview question.

---

# ANOTHER HUGE QUESTION

---

# QUESTION

```js id="jlwmwy"
console.log(Object.prototype.__proto__);
```

Output:

```text id="jlwmxv"
null
```

Why?

Because Object.prototype is top of standard object chain.

---

# SENIOR-LEVEL UNDERSTANDING

You should now understand:

* JS inheritance is prototype delegation
* objects inherit dynamically
* property lookup climbs prototype chain
* methods are shared through prototypes
* classes are syntax sugar
* `new` links prototype relationships
* memory efficiency comes from shared methods

---

# MOST IMPORTANT TAKEAWAYS

---

# Objects contain hidden prototype links

```text id="jlwmyr"
[[Prototype]]
```

---

# Functions contain prototype property

```text id="jlwmzn"
prototype
```

---

# Property lookup climbs upward

```text id="jlwn0k"
Object → Prototype → Parent Prototype → null
```

---

# JavaScript inheritance is NOT class copying

It is:

# Prototype Delegation

That is the true heart of JS inheritance.
