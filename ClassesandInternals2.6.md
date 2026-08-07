# PHASE 2.6 — CLASSES INTERNALS (Deep Dive)

Now you’ll understand something extremely important:

# JavaScript classes are NOT real classical classes internally.

They are mostly:

```text id="3d8fzt"
Syntax sugar over prototypes
```

This is one of the biggest JavaScript interview concepts.

Many developers think:

```text id="6v7hjk"
JS classes work like Java classes
```

WRONG.

Internally JS still uses:

* objects
* functions
* prototype chains
* delegation

Classes only provide cleaner syntax.

---

# FIRST BIG IDEA

Before ES6:

JavaScript used:

```js id="25d3k8"
function User() {}
```

After ES6:

```js id="6i0s9y"
class User {}
```

But internally:

```text id="k2f0xq"
Still prototype system
```

---

# SIMPLE CLASS EXAMPLE

```js id="lb74y0"
class User {
  constructor(name) {
    this.name = name;
  }

  greet() {
    console.log("Hello");
  }
}
```

Looks modern.

But internally roughly becomes:

---

# INTERNAL APPROXIMATION

```js id="8e5m2v"
function User(name) {
  this.name = name;
}

User.prototype.greet = function() {
  console.log("Hello");
};
```

Almost same behavior.

---

# HUGE INSIGHT

Class methods are NOT copied into every object.

They live in:

```text id="2q9e1w"
User.prototype
```

Shared across all instances.

Memory efficient.

---

# WHAT HAPPENS WHEN OBJECT CREATED

---

# Example

```js id="q8u4a6"
const u1 = new User("Musharaf");
```

---

# INTERNAL STEPS

---

# STEP 1

Creates empty object.

```text id="7z2p9l"
{}
```

---

# STEP 2

Links prototype.

```text id="m1d7s5"
u1.[[Prototype]]
     ↓
User.prototype
```

---

# STEP 3

Binds:

```text id="9k3x2q"
this → new object
```

inside constructor.

---

# STEP 4

Executes constructor.

```js id="8f4j1u"
this.name = name
```

---

# STEP 5

Returns new object.

---

# FINAL STRUCTURE

```text id="u3v8c0"
u1
 ↓
User.prototype
 ↓
Object.prototype
 ↓
null
```

---

# CLASS METHODS LIVE IN PROTOTYPE

---

# Example

```js id="l0x9b4"
class User {
  greet() {
    console.log("Hello");
  }
}
```

Internally:

```text id="5n7y2w"
User.prototype.greet
```

NOT:

```text id="3k1q8e"
u1.greet
```

---

# PROOF

```js id="c7v2m1"
const u1 = new User();
const u2 = new User();

console.log(u1.greet === u2.greet);
```

Output:

```text id="y9w4t6"
true
```

Because SAME shared function.

Huge memory optimization.

---

# CONSTRUCTOR METHOD

Special method:

```js id="f2m8x5"
constructor()
```

Automatically runs during:

```js id="1k7z9q"
new User()
```

---

# IMPORTANT RULE

Only ONE constructor allowed.

---

# DEFAULT CONSTRUCTOR

If absent:

```js id="v5q1w3"
class User {}
```

JS internally creates empty constructor.

---

# INSTANCE PROPERTIES VS PROTOTYPE METHODS

Critical distinction.

---

# INSTANCE PROPERTIES

Created individually per object.

Example:

```js id="z6p2x8"
constructor(name) {
  this.name = name;
}
```

Each instance gets own copy.

---

# PROTOTYPE METHODS

Shared.

Example:

```js id="m9x4c7"
greet() {}
```

Lives once in prototype.

---

# WHY THIS DESIGN?

Because methods usually same behavior.

No need duplicate memory.

---

# CLASS INHERITANCE

Now VERY important.

---

# Example

```js id="q1w8z4"
class Animal {
  eat() {
    console.log("Eating");
  }
}

class Dog extends Animal {}
```

---

# WHAT DOES `extends` DO?

Internally sets prototype chain.

---

# STRUCTURE

```text id="8x2m6q"
Dog.prototype
      ↓
Animal.prototype
      ↓
Object.prototype
```

---

# PROPERTY LOOKUP

```js id="t7v3p1"
const d = new Dog();

d.eat();
```

Search:

---

# STEP 1

Inside d object.

No `eat`.

---

# STEP 2

Dog.prototype.

No `eat`.

---

# STEP 3

Animal.prototype.

Found.

Execute.

---

# THIS IS INHERITANCE

Through prototype delegation.

NOT copying methods.

---

# SUPER KEYWORD

Critical concept.

---

# Example

```js id="k3m7x2"
class Animal {
  constructor(name) {
    this.name = name;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);

    this.breed = breed;
  }
}
```

---

# WHAT DOES `super()` DO?

Calls parent constructor.

Equivalent roughly:

```js id="m6x9v1"
Animal.call(this, name)
```

---

# IMPORTANT RULE

In child constructor:

```text id="u2p8z7"
super() must run before this
```

Otherwise error.

---

# WHY?

Because parent initializes object first.

Until then:

```text id="d7x4k3"
this not ready
```

---

# METHOD OVERRIDING

---

# Example

```js id="f1v9q5"
class Animal {
  speak() {
    console.log("Animal");
  }
}

class Dog extends Animal {
  speak() {
    console.log("Dog");
  }
}
```

---

# LOOKUP

```js id="n4x2m8"
const d = new Dog();

d.speak();
```

Search finds:

```text id="b7k5q1"
Dog.prototype.speak
```

first.

Stops there.

Parent method shadowed.

---

# CALLING PARENT METHOD

---

# Example

```js id="q8m1v6"
class Dog extends Animal {
  speak() {
    super.speak();

    console.log("Dog");
  }
}
```

`super.speak()` means:

```text id="x4v7z9"
Look in parent prototype
```

---

# STATIC METHODS

VERY IMPORTANT.

---

# Example

```js id="m9q3x1"
class User {
  static hello() {
    console.log("Hello");
  }
}
```

---

# CALL

```js id="v2z8k4"
User.hello();
```

Works.

---

# BUT

```js id="n6x1q7"
const u = new User();

u.hello();
```

Error.

---

# WHY?

Static methods belong to:

```text id="f3m7v9"
Class itself
```

NOT instances.

---

# INTERNAL VIEW

```text id="q1x8k5"
User.hello
```

NOT:

```text id="d4v2m6"
User.prototype.hello
```

Huge difference.

---

# PRIVATE FIELDS

Modern JS feature.

---

# Example

```js id="k7x3m1"
class Bank {
  #balance = 0;

  deposit(v) {
    this.#balance += v;
  }
}
```

---

# IMPORTANT

```js id="x2m8q5"
obj.#balance
```

outside class = SyntaxError.

---

# HOW PRIVATE FIELDS DIFFER FROM NORMAL PROPERTIES

Normal properties:

```js id="z9q4v1"
this.balance
```

publicly accessible.

Private fields stored differently internally.

---

# GETTERS AND SETTERS

---

# Example

```js id="m5v1x7"
class User {
  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name;
  }

  set name(v) {
    this._name = v;
  }
}
```

---

# USAGE

```js id="q8x2m4"
u.name
```

Looks like property access.

But internally executes function.

Huge OOP concept.

---

# CLASS HOISTING

Interesting interview topic.

---

# FUNCTION DECLARATION

```js id="v1m7q8"
test();

function test() {}
```

Works.

---

# CLASS

```js id="m3x8q1"
const u = new User();

class User {}
```

Error.

---

# WHY?

Classes are hoisted BUT exist in:

```text id="q6v2m9"
Temporal Dead Zone
```

similar to let/const.

---

# IMPORTANT DIFFERENCE FROM JAVA

Java:

```text id="w4m8q3"
True class-based inheritance
```

JavaScript:

```text id="x7v1m5"
Prototype delegation hidden behind class syntax
```

Massive conceptual difference.

---

# HUGE INTERVIEW QUESTION

---

# QUESTION

```js id="q2m9x6"
class A {}

console.log(typeof A);
```

Output?

```text id="m5v7q1"
function
```

WHY?

Because classes internally are special functions.

---

# ANOTHER HUGE QUESTION

---

# QUESTION

```js id="x1m8q4"
class User {
  greet() {}
}

console.log(typeof User.prototype.greet);
```

Output:

```text id="v9q2m7"
function
```

Methods still functions stored in prototype.

---

# CLASS FIELDS VS METHODS

---

# Example

```js id="k3q7m1"
class User {
  name = "A";

  greet() {}
}
```

---

# IMPORTANT

`name` becomes instance property.

`greet` becomes prototype method.

Different memory behavior.

---

# HUGE PERFORMANCE INSIGHT

Methods inside constructor/class fields:

```js id="q6m1x8"
greet = () => {}
```

create NEW function per instance.

Can increase memory usage.

Prototype methods more memory efficient.

Very important React/class component insight historically.

---

# SENIOR-LEVEL UNDERSTANDING

You should now understand:

* classes are syntax sugar
* inheritance still prototype-based
* methods shared through prototypes
* extends links prototype chains
* super accesses parent behavior
* static methods belong to class
* instance fields belong to objects
* method lookup follows prototype delegation

---

# MOST IMPORTANT TAKEAWAYS

---

# JavaScript classes are internally functions

---

# Methods live on prototype

---

# Inheritance works via prototype chains

---

# `extends` modifies prototype linkage

---

# `super` accesses parent prototype/constructor

---

# JS OOP is NOT classical internally

It is still:

# Prototype-Based Inheritance hidden behind cleaner syntax.
