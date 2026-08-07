They do this:

```
charAt()
slice()
substring()
substr()
split()
replace()
replaceAll()
...
```

---

# Method 1.3 — String Operations (Senior Mental Model)

## Rule No. 1

Before thinking about methods, ask:

```
What am I trying to do with this string?
```

Almost every string problem belongs to one of these categories.

```
1. Read
2. Search
3. Extract
4. Modify
5. Validate
6. Convert
7. Compare
8. Format
```

When you know the category, the possible methods become obvious.

---

# Category 1 — READ

Question:

> "I only want information."

Notice something important.

You are **not changing** the string.

You're just reading from it.

Example

```js
const name = "Musharaf";
```

Possible questions

```
How many letters?

First letter?

Last letter?

Character at index 3?
```

These are **Read Operations**.

---

## Method 1

```js
length
```

Question

```
How long is this string?
```

Example

```js
const name = "Musharaf";

console.log(name.length);
```

Output

```
8
```

Why isn't it

```js
length()
```

Because JavaScript already knows the length.

It doesn't need to calculate anything.

Imagine a library book.

```
Book

Pages = 250
```

The number already exists.

You're simply reading it.

---

Senior thinking

```
Need information

↓

Property

↓

length
```

---

## Method 2

```js
[]
```

Question

```
Need character at position?
```

Example

```js
const name = "Musharaf";

console.log(name[0]);
```

Output

```
M
```

Another

```js
console.log(name[3]);
```

Output

```
h
```

---

Question

Why does indexing start at 0?

This is not unique to JavaScript.

Most programming languages use zero-based indexing because it maps directly to memory offsets.

Imagine memory:

```
M u s h a r a f

Offset

0
1
2
3
4
5
6
7
```

The first character is **0 characters away** from the start.

So its index is 0.

---

## Question

Can we do

```js
name[100]
```

Yes.

Output

```js
undefined
```

No error.

Why?

Because JavaScript checks.

```
Does index exist?

↓

No

↓

Return undefined
```

---

## Method 3

```js
charAt()
```

Example

```js
name.charAt(0)
```

Output

```
M
```

Looks same as

```js
name[0]
```

So why does it exist?

History.

Years ago,

JavaScript didn't support

```js
name[0]
```

Developers used

```js
charAt()
```

Now both work.

Senior developers usually prefer

```js
name[0]
```

because it's shorter and easier to read.

---

Interesting difference

```js
name[100]
```

returns

```js
undefined
```

But

```js
name.charAt(100)
```

returns

```text
""
```

An empty string.

Interview question.

---

# Category 2 — SEARCH

Question

```
Does this text exist?

Where is it?

How many times?

Does it start with...

Does it end with...
```

Notice

You're still **not changing** the string.

You're only searching.

---

## includes()

Question

```
Contains?
```

Example

```js
const email = "abc@gmail.com";

email.includes("@");
```

Output

```
true
```

Senior thinking

```
Need yes/no

↓

includes()
```

---

Example

```js
email.includes("gmail")
```

Output

```
true
```

---

Example

```js
email.includes("yahoo")
```

Output

```
false
```

---

Why not use

```js
indexOf()
```

We'll answer that shortly.

---

## startsWith()

Question

```
Starts with?
```

Example

```js
const url = "https://google.com";

url.startsWith("https");
```

Output

```
true
```

---

Real-world use

```
Protocol validation

File extension

Prefixes

API routes
```

---

## endsWith()

Question

```
Ends with?
```

Example

```js
const file = "resume.pdf";

file.endsWith(".pdf");
```

Output

```
true
```

Real projects use this constantly.

---

## indexOf()

This is one beginners misuse a lot.

Question

```
Where is this text?
```

Example

```js
const email = "abc@gmail.com";

email.indexOf("@");
```

Output

```
3
```

Why 3?

```
a 0
b 1
c 2
@ 3
```

---

Now

```js
email.indexOf("x")
```

Output

```
-1
```

Why not

```
false
```

Because

`indexOf()` isn't answering

> Does it exist?

It's answering

> Where is it?

If it doesn't exist,

there is no valid position.

So JavaScript returns

```
-1
```

---

Senior rule

Need

```
Yes/No

↓

includes()
```

Need

```
Position

↓

indexOf()
```

Don't use

```js
indexOf(...) !== -1
```

unless you actually need the position.

Modern JavaScript introduced `includes()` specifically to make that intent clearer.

---

# Category 3 — EXTRACT

Question

```
Need only part of the string.
```

Example

```
hello@gmail.com

↓

hello
```

Need extraction.

---

There are three famous methods.

```
slice()

substring()

substr()
```

This confuses everyone.

Let's understand **why** all three exist.

---

## slice()

Think of it as a knife.

```
ABCDEFG

Cut

2 → 5
```

Result

```
CDE
```

Example

```js
const word = "ABCDEFG";

word.slice(2,5);
```

Output

```
CDE
```

Notice

End index isn't included.

```
2

3

4

Stop before 5
```

---

Why?

Because JavaScript treats the second number as

```
Ending boundary

Not included
```

Exactly like

```
for(let i=2; i<5; i++)
```

Notice

Not

```
<=5
```

Same idea.

---

## Why is slice() So Popular?

Because it supports negative indexes.

Example

```js
const word = "ABCDEFG";

word.slice(-2);
```

Output

```
FG
```

Amazing.

Count from the end.

```
A B C D E F G

-7 -6 -5 -4 -3 -2 -1
```

Senior developers love `slice()` because of this flexibility.

---

## substring()

Looks similar.

```js
substring(2,5)
```

Same output.

But...

Negative values become 0.

```js
substring(-2)
```

Actually behaves like:

```js
substring(0)
```

Not what many expect.

That's why most modern code prefers `slice()`.

---

## substr()

Example

```js
substr(2,3)
```

Meaning

```
Start at 2

Take 3 characters
```

Not

```
Start

End
```

Different meaning.

Today it's considered legacy and is deprecated in many environments.

As a senior developer, avoid using it in new code.

---

# Senior Decision

Need extraction?

```
Use slice()
```

99% of the time.

It's expressive, supports negative indexes, and is widely preferred.

---

# Mini Challenge (Think Like a Senior)

Suppose you receive:

```js
const filename = "resume.pdf";
```

You need only:

```
resume
```

Don't think:

> Which method?

Think:

```
Input

↓

String

Need

↓

Part of string

Operation

↓

Extract

↓

Possible methods

slice()

substring()

Which is more flexible?

↓

slice()
```

This shift—from memorizing methods to recognizing operations—is exactly how experienced developers approach problems.

---

# Today's Progress

Today you learned to classify string work into three major categories:

| Category | Purpose              | Common Tools                                                   |
| -------- | -------------------- | -------------------------------------------------------------- |
| Read     | Get information      | `length`, `[]`, `charAt()`                                     |
| Search   | Find text            | `includes()`, `startsWith()`, `endsWith()`, `indexOf()`        |
| Extract  | Get part of a string | `slice()`, `substring()` (`substr()` for historical knowledge) |

Notice we still haven't tried to memorize a long list. Instead, you're building a decision tree.

---

# Coming in Method 1.4

We'll cover the remaining categories that you'll use constantly in real applications:

* **Modify** (`replace`, `replaceAll`, `trim`, `padStart`, `padEnd`, etc.)
* **Validate** (email checks, passwords, prefixes, suffixes, whitespace)
* **Convert** (`split`, `String()`, template literals, numbers ↔ strings)
* **Compare** (`===`, `localeCompare`, case-insensitive comparison)
* **Format** (uppercase, lowercase, capitalization, normalization)

We'll also discuss common interview questions, performance considerations, and why senior developers choose one method over another in production code.
