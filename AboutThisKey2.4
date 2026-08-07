# PHASE 2.4 — `this` KEYWORD (Deep Dive)

This is one of the MOST confusing JavaScript concepts.

Why?

Because:

```text id="6u8v9n"
this changes dynamically
```

depending on:

* how function is called
* execution context
* strict mode
* arrow functions
* objects
* classes
* events
* bind/call/apply

Most languages:

```text id="f7w2qx"
this = current object
```

JavaScript:

```text id="l9x2wr"
Not that simple
```

In JS:

# `this` depends on HOW function is called

NOT where function is written.

This is the biggest rule.

---

# SIMPLE DEFINITION

`this` is:

```text id="t4pq0v"
A special keyword referring to execution context owner
```

Better understanding:

> `this` points to the object that is currently calling the function.

But even this has exceptions.

---

# MOST IMPORTANT RULE

Do NOT think:

```text id="0tqfxf"
"this belongs to function"
```

WRONG.

Instead:

```text id="l0ktm1"
this is determined during function INVOCATION
```

This changes everything.

---

# CASE 1 — GLOBAL CONTEXT

---

# Browser

```js id="g7c92k"
console.log(this);
```

Output:

```text id="0uxfx8"
window object
```

Because global execution context owner is `window`.

---

# Node.js

Different.

Top-level `this` behaves differently.

---

# CASE 2 — REGULAR FUNCTION

---

# Example

```js id="j3tq4z"
function test() {
  console.log(this);
}

test();
```

---

# NON-STRICT MODE

Browser output:

```text id="k9xq2n"
window object
```

WHY?

Because plain function call defaults to global object.

---

# STRICT MODE

```js id="m7x2qp"
"use strict";

function test() {
  console.log(this);
}

test();
```

Output:

```text id="z0qx2m"
undefined
```

---

# WHY?

Strict mode prevents accidental global binding.

Safer behavior.

---

# HUGE INSIGHT

Function itself does NOT decide `this`.

CALL STYLE decides.

---

# CASE 3 — OBJECT METHOD

---

# Example

```js id="q7t9xn"
const user = {
  name: "Musharaf",

  greet() {
    console.log(this.name);
  }
};

user.greet();
```

Output:

```text id="p0w9qa"
Musharaf
```

---

# WHY?

Because:

```text id="d4v7mk"
user object called function
```

Thus:

```text id="y5k8xn"
this → user
```

---

# IMPORTANT INTERNAL VIEW

This:

```js id="m2q9tj"
user.greet()
```

is NOT same as:

```js id="f9w8zp"
greet()
```

Dot before function call matters hugely.

---

# VERY IMPORTANT INTERVIEW QUESTION

---

# QUESTION

```js id="t1q7zy"
const user = {
  name: "A",

  greet() {
    console.log(this.name);
  }
};

const fn = user.greet;

fn();
```

Output?

---

# NON-STRICT MODE

```text id="u8k3pv"
undefined
```

(or global object behavior)

---

# WHY?

Because now function called as:

```js id="y0v7xr"
fn()
```

NOT:

```js id="e2x9qw"
user.greet()
```

So object ownership lost.

This is MASSIVE concept.

---

# KEY RULE

```text id="n5x8ua"
Who CALLS function determines this
```

NOT where function originally came from.

---

# CASE 4 — NESTED OBJECTS

---

# Example

```js id="p8t1qm"
const obj = {
  name: "Outer",

  inner: {
    name: "Inner",

    show() {
      console.log(this.name);
    }
  }
};

obj.inner.show();
```

Output:

```text id="v2m7qk"
Inner
```

Because caller:

```text id="s1q8yv"
obj.inner
```

---

# CASE 5 — ARROW FUNCTIONS

EXTREMELY IMPORTANT.

Arrow functions behave COMPLETELY differently.

---

# BIG RULE

Arrow functions do NOT have their own `this`.

Instead:

```text id="g7n9pw"
They inherit this from surrounding lexical scope
```

This is HUGE.

---

# EXAMPLE

```js id="u7t1mp"
const user = {
  name: "Musharaf",

  greet: () => {
    console.log(this.name);
  }
};

user.greet();
```

Output:

```text id="q0v9xn"
undefined
```

WHY?

Because arrow function ignores object caller.

It lexically captures surrounding `this`.

---

# IMPORTANT

Arrow functions use:

```text id="y6p2qw"
Lexical this
```

Similar to closures.

---

# VISUAL

Regular function:

```text id="v4x8mk"
this determined at CALL TIME
```

Arrow function:

```text id="h3q9zy"
this determined at CREATION TIME
```

Massive difference.

---

# WHY ARROW FUNCTIONS EXIST

To solve old JS problems.

Especially callbacks.

---

# CLASSIC PROBLEM

```js id="m1q8tp"
const user = {
  name: "A",

  greet() {
    setTimeout(function () {
      console.log(this.name);
    }, 1000);
  }
};

user.greet();
```

Output:

```text id="x8v2qm"
undefined
```

---

# WHY?

Callback executes as plain function:

```js id="z2t9qy"
function()
```

So:

```text id="d7p1xm"
this → window / undefined
```

NOT user.

---

# FIX WITH ARROW FUNCTION

```js id="n5q8tw"
const user = {
  name: "A",

  greet() {
    setTimeout(() => {
      console.log(this.name);
    }, 1000);
  }
};
```

Output:

```text id="k1v7qp"
A
```

---

# WHY?

Arrow function captures `this` from greet() context.

Inside greet():

```text id="z6x9tw"
this → user
```

Arrow inherits it.

---

# HUGE REACT INSIGHT

This is why arrow functions popular in React.

Avoid losing `this`.

---

# CASE 6 — CONSTRUCTOR FUNCTIONS

---

# Example

```js id="r8q2tm"
function User(name) {
  this.name = name;
}

const u1 = new User("Musharaf");

console.log(u1.name);
```

Output:

```text id="t0v8xn"
Musharaf
```

---

# WHAT DOES `new` DO?

Internally:

---

# STEP 1

Creates empty object.

```text id="g2q9xm"
{}
```

---

# STEP 2

Sets prototype link.

---

# STEP 3

Binds:

```text id="p1v7qt"
this → new object
```

---

# STEP 4

Executes constructor.

```js id="u3x8qp"
this.name = name
```

becomes:

```js id="h6q2tw"
newObject.name = name
```

---

# STEP 5

Returns object.

---

# THIS IS WHY

```js id="q9t1xp"
new User()
```

works.

---

# CASE 7 — call apply bind

VERY IMPORTANT INTERVIEW TOPIC.

These methods manually control `this`.

---

# call()

Executes immediately.

---

# Example

```js id="p4q8tn"
function greet() {
  console.log(this.name);
}

const user = {
  name: "Musharaf"
};

greet.call(user);
```

Output:

```text id="v7x2qm"
Musharaf
```

---

# WHY?

call() explicitly sets:

```text id="t2q9xp"
this → user
```

---

# apply()

Same as call.

Difference:

Arguments passed as array.

---

# bind()

Does NOT execute immediately.

Returns new function with fixed `this`.

---

# Example

```js id="r1v8qw"
const boundFn = greet.bind(user);

boundFn();
```

Output:

```text id="x5q2tm"
Musharaf
```

---

# HUGE INTERVIEW QUESTION

---

# QUESTION

```js id="n7t1qp"
const user = {
  name: "A",

  greet() {
    console.log(this.name);
  }
};

setTimeout(user.greet, 1000);
```

Output?

```text id="z8v2qw"
undefined
```

---

# WHY?

Because function reference passed.

Later executes independently.

Object context lost.

---

# FIX

```js id="f2q9xm"
setTimeout(user.greet.bind(user), 1000);
```

---

# ANOTHER HUGE INSIGHT

`this` is NOT lexical in regular functions.

Meaning:

```text id="d9t1qp"
this ignores scope chain
```

Completely separate mechanism.

---

# IMPORTANT DIFFERENCE

Variables:

```text id="x7q2tw"
Resolved lexically
```

`this`:

```text id="v3q8xm"
Resolved dynamically
```

Massive conceptual difference.

---

# INTERVIEW TRICK QUESTION

---

# QUESTION

```js id="w1t9qp"
const obj = {
  name: "A",

  arrow: () => {
    console.log(this.name);
  },

  regular() {
    console.log(this.name);
  }
};

obj.arrow();
obj.regular();
```

Output?

---

# arrow()

```text id="r6q2tw"
undefined
```

because arrow captures outer/global this.

---

# regular()

```text id="m8v1qp"
A
```

because regular function uses caller object.

---

# SENIOR-LEVEL UNDERSTANDING

You should now understand:

* `this` is runtime-bound
* regular functions create dynamic `this`
* arrow functions capture lexical `this`
* object ownership matters
* call/apply/bind manually override context
* callbacks often lose `this`
* React historically used arrows to avoid binding problems

---

# FINAL GOLDEN RULES

---

# REGULAR FUNCTION

```text id="g0q9xm"
this = determined by HOW function called
```

---

# ARROW FUNCTION

```text id="p7v2tw"
this = inherited from surrounding scope
```

---

# MOST IMPORTANT THING TO REMEMBER

JavaScript has TWO separate systems:

---

# VARIABLES

Use:

```text id="y1q8qp"
Lexical Scope Chain
```

---

# this

Uses:

```text id="h4v9xm"
Invocation Context
```

Completely different mechanisms.
