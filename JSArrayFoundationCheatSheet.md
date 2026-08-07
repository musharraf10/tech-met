# JavaScript Array Foundation (Senior Cheat Sheet)

## 1. What is an Array?

An Array is an **ordered collection of values**.

```js
const arr = [10, 20, 30];
```

Think:

```
Index
0 → 10
1 → 20
2 → 30
```

Purpose:

* Store multiple values
* Preserve order
* Fast index access

---

# 2. Arrays are Objects

Many developers don't know this.

```js
const arr = [1,2,3];

typeof arr
```

Output

```
object
```

Because internally

```
Array

↓

Special Object

↓

Extra Features

length
push()
pop()
map()
filter()
...
```

---

# 3. Why Array Instead of Variables?

Instead of

```js
let a=10;
let b=20;
let c=30;
```

Use

```js
const arr=[10,20,30];
```

Now

* Loop
* Search
* Sort
* Filter
* Update

becomes easy.

---

# 4. Index Starts at 0

```
A B C

0 1 2
```

Reason

First element is **0 positions away** from start.

Most languages follow this.

---

# 5. length

```
Information

↓

Property
```

Not a method.

```js
arr.length
```

Returns

```
Number
```

---

# 6. Mutable vs Immutable

## Mutable

Changes original array.

```
push
pop
shift
unshift
sort
reverse
splice
fill
copyWithin
```

---

## Immutable

Returns new array.

```
slice
map
filter
concat
flat
flatMap
toSorted
toReversed
toSpliced
with
```

---

Senior rule

Before using a method ask

```
Does this modify original?
```

---

# 7. push()

```
Add

↓

End
```

```js
arr.push(10)
```

Returns

```
New length
```

Why?

JavaScript assumes after insertion you may want length.

Complexity

```
O(1)
```

Usually very fast.

---

# 8. pop()

```
Remove

↓

Last
```

Returns

```
Removed element
```

Complexity

```
O(1)
```

---

# 9. shift()

```
Remove

↓

First
```

Complexity

```
O(n)
```

Why?

Every element shifts left.

```
Before

0 A
1 B
2 C

After

0 B
1 C
```

Everything moves.

---

# 10. unshift()

```
Add

↓

Beginning
```

Complexity

```
O(n)
```

Everything shifts right.

---

# 11. slice()

```
Copy part

↓

New Array
```

Original unchanged.

```
0 1 2 3 4

slice(1,4)

↓

1 2 3
```

---

# 12. splice()

Swiss Army knife.

Can

```
Insert

Delete

Replace
```

Changes original.

Interview favorite.

---

# 13. Search Family

Need

```
Exists?

↓

includes()
```

Returns

```
Boolean
```

---

Need

```
Position?

↓

indexOf()
```

Returns

```
Index

or

-1
```

---

Need

```
Object?

↓

find()
```

Returns

```
Object

or

undefined
```

---

Need

```
Object Position?

↓

findIndex()
```

Returns

```
Index
```

---

Need

```
Many objects?

↓

filter()
```

Returns

```
Array
```

---

# 14. map()

Purpose

```
Transform
```

Input

```
Array
```

Output

```
Array
```

Same length.

Example

```
Prices

↓

GST Added

↓

New Prices
```

Never use map when not returning values.

---

# 15. filter()

Purpose

```
Keep

Discard
```

Output

```
Smaller Array
```

Example

```
Users

↓

Only Active
```

---

# 16. reduce()

Purpose

```
Many

↓

One
```

Examples

```
Sum

Average

Maximum

Minimum

Grouping

Frequency Count
```

Returns

Anything.

Number

Object

Array

String

Depends on accumulator.

---

# 17. forEach()

Purpose

```
Do something
```

No returned array.

Examples

```
Console

API

Database

Logging
```

Never use forEach expecting new array.

---

# 18. some()

Question

```
Any?
```

Returns

Boolean.

Stops at first true.

Example

```
Any Admin?
```

---

# 19. every()

Question

```
All?
```

Returns

Boolean.

Stops at first false.

Example

```
Every User Verified?
```

---

# 20. sort()

Default

```
Alphabetical

NOT Numeric
```

Example

```js
[10,2,5].sort()
```

Output

```
10

2

5
```

Need comparator.

```js
a-b
```

---

# 21. reverse()

Changes original.

Modern

```
toReversed()
```

Returns copy.

---

# 22. concat()

Merge arrays.

Original unchanged.

---

# 23. join()

Array

↓

String

```
A B C

↓

A-B-C
```

Opposite of

```
split()
```

---

# 24. flat()

Nested

↓

Single Level

```
[[1],[2]]

↓

[1,2]
```

---

# 25. flatMap()

```
map

+

flat
```

One pass.

---

# 26. Spread (...)

Copy

Merge

Insert

```js
[...arr]
```

Shallow copy.

---

# 27. Reference Problem

```js
const a=[1,2];

const b=a;
```

Both point same array.

```
a

↓

Memory

↑

b
```

Changing

```js
b.push(5)
```

Also changes

```
a
```

---

Need copy

```js
const b=[...a];
```

---

# 28. Shallow Copy

Copies first level only.

Nested objects still shared.

```
Array

↓

Object

↓

Same Reference
```

---

Need deep copy

```
structuredClone()
```

---

# 29. Array Decision Tree

Need

```
Transform

↓

map()
```

Need

```
Remove Items

↓

filter()
```

Need

```
One Item

↓

find()
```

Need

```
Index

↓

findIndex()
```

Need

```
Any?

↓

some()
```

Need

```
All?

↓

every()
```

Need

```
Sum

↓

reduce()
```

Need

```
Loop Only

↓

forEach()
```

Need

```
Copy

↓

slice()

spread
```

Need

```
Merge

↓

concat()

spread
```

Need

```
Insert End

↓

push()
```

Need

```
Remove End

↓

pop()
```

Need

```
Insert Anywhere

↓

splice()
```

Need

```
Sort

↓

sort()

toSorted()
```

Need

```
Flatten

↓

flat()
```

---

# 30. Time Complexity (Must Know)

| Method       | Complexity |
| ------------ | ---------- |
| `arr[index]` | O(1)       |
| `push()`     | O(1)       |
| `pop()`      | O(1)       |
| `shift()`    | O(n)       |
| `unshift()`  | O(n)       |
| `find()`     | O(n)       |
| `filter()`   | O(n)       |
| `map()`      | O(n)       |
| `reduce()`   | O(n)       |
| `includes()` | O(n)       |
| `indexOf()`  | O(n)       |
| `slice()`    | O(n)       |
| `concat()`   | O(n)       |
| `sort()`     | O(n log n) |
| `reverse()`  | O(n)       |

---

# 31. Pure vs Impure

Pure (No mutation)

```
map

filter

slice

concat

flat

toSorted
```

Impure (Mutation)

```
push

pop

shift

unshift

splice

sort

reverse
```

---

# 32. Return Types (Senior Trick)

Need

```
Question

↓

Boolean
```

Methods

```
includes

some

every
```

Need

```
One Value

↓

find

pop

shift
```

Need

```
Many Values

↓

Array
```

Methods

```
filter

map

slice

concat
```

Need

```
One Final Result

↓

reduce
```

Need

```
Position

↓

Number
```

Methods

```
indexOf

findIndex
```

---

# 33. Real Project Usage

React UI

```
map()
```

Search

```
filter()
```

Shopping Cart Total

```
reduce()
```

Authentication

```
find()
```

Permission Check

```
some()
```

Form Validation

```
every()
```

Pagination

```
slice()
```

Sorting Products

```
sort()
```

Merge API Data

```
concat()

spread
```

---

# 34. Senior Mental Model

Never memorize methods.

Always think:

```
Current Type
      │
      ▼
Array
      │
      ▼
What is my goal?
      │
      ├── Read → [] length at()
      ├── Search → includes find indexOf
      ├── Add → push unshift splice
      ├── Remove → pop shift splice filter
      ├── Transform → map
      ├── Filter → filter
      ├── Aggregate → reduce
      ├── Validate → some every
      ├── Sort → sort toSorted
      ├── Copy → slice spread
      ├── Merge → concat spread
      ├── Convert → join
      └── Flatten → flat flatMap
```

This is the mental model many experienced JavaScript developers use. They don't start by asking **"Which method do I remember?"** They start by asking **"What operation am I performing?"** Once the operation is clear, the appropriate method usually becomes obvious.
