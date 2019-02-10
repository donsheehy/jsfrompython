# Javascript for Python Programmers

## Introduction

The goal of this little document is to get Python programmers up and running with JavaScript.
The basic challenges faced by anyone learning a programming language are
- run code
- write expressions
- store data in variables
- use builtin data structures
- if-then-else
- functions
- loops
- define classes


## Beginning to play

There is a strong case to made that the proper place for JavaScript to run is in a browser.
However, if we want to build up from the basics, it will help a good deal to be able to run code from the commandline and also in an interactive shell.
For Python, this is perhaps the most natural thing.
One runs code from the commandline as follows.

```bash
$ python myprogram.py
```

Moreover, to access an interactive Python shell, one merely writes the following.

```bash
$ python
```

Now, we'd like to do the same with JavaScript in order to get started.
For this, we will use `node`.
In some places, you will see node listed as an advanced topic.
It does require you to download and install it (https://nodejs.org/en/), which is a bit more work than is required if you just run your code in a browser.
Once installed, you can use `node` the same way we used `python` before.

```bash
$ node myprogram.js
```

Or, to access an interactive shell, we do the following on the commandline.

```bash
$ node
```

## Defining variables and doing some arithmetic

The starting point for many simple programs is to define some variables and do some arithmetic.  Here is some Python.

```python {cmd}
x = 11
y = (x + 7) * 100
print(y)
```

Here is the same thing in JavaScript.

```node {cmd}
let x = 11;
let y = (x + 7) * 100;
console.log(y);
```

There are only a couple differences to note.
First, the variables are defined using the `var` keyword.
In these cases, one could also use `let` (and maybe should in modern programs) as in the following.

```node {cmd}
let x = 11;
let y = (x + 7) * 100;
console.log(y);
```

There are also semicolons at the end of the lines.
The semicolons are not entirely necessary, but they are sometimes needed to separate statements.
One can get away without them in most cases, but be aware that you will likely encounter JavaScript programmers in the wild that feel *very* strongly about their use.

The last line is `console.log`.
This is the basic way to print output in JS.

Lastly, we can also store data in constants, declared using the `const` keyword.
Be aware that constants are not necessarily immutable, they are merely names that cannot be reassigned.
So, the following works just fine.

```node {cmd}
const x = 11;
const y = (x + 7) * 100;
console.log(y);
```

The following gives an error.

```node {cmd}
const x = 11
x = 12
```

## Basic Types



## Some builtin data structures

Python programmers get great mileage out of the `list` and `dict` types.
These two structures are so useful in so many situations, I frankly find it uncomfortable to stray far from Python and leave these behind.
Still, one keeps a fair amount of this power in the JavaScript world.
The Python `list` corresponds loosely to the JavaScript `array`.
The Python `dict` corresponds (more) loosely to the JavaScript `Object`.
Mercifully, the syntax for both `array` and `Object` are quite similar to their Python counterparts.

### Arrays

In Python, a simple sequential collection is usually stored as a `list`.
A list literal is delimited by square brackets (`[]`).

```python {cmd}
L = [3, 5, 1000]
print(L)
```

In JS, a similar functionality is available using the builtin `Array` type.

```node {cmd}
let A  = [3, 5, 1000];
console.log(A);
```

In both languages, it is permissible to include items of heterogeneous types.
Moreover, both languages use square brackets to access items by their (zero-based) index.
Assignment is also similar.
*Don't use negative indices in JS.*

```python {cmd}
L = [3, 5.1, 'a string!']
L[2] = 'an updated string'
print(L)
print(L[0])
print(L[-1])
```

```node {cmd}
let A  = [3, 5.1, 'a string!'];
A[2] = 'an updated string';
console.log(A);
console.log(A[0]);
console.log(A[2]);
console.log(A[-1]);
```

<!-- length, append/push, pop, slice, copy -->
Here is an example that creates a list, appends items, pops items, accesses the length, slices the list, copies it, and compares it to another list.

```python {cmd}
L = ['a', 14, 100]

L.append(101)
L_slice = L[1: 3]
print("Length of L is", len(L))
print("Length of L_slice is", len(L_slice))

print("Last item is", L.pop())
print("New length of L (after the pop) is", len(L))

L_copy = L[:]
assert(L == L_copy)
L[0] = 'a totally new value'
assert(L != L_copy)
```

Almost all of these operations are available directly in JS, albeit without the elegant notation.
Checking equality requires an explicit loop.

```node {cmd}
const A = ['a', 14, 100];

A.push(101);
A_slice = A.slice(1,3);
console.log("Length of A is " + A.length)
console.log("Length of A_slice is", A_slice.length)

console.log("Last item is", A.pop())
console.log("New length of A (after the pop) is", A.length)

A_copy = A.slice()
for (i = 0; i < A.length; ++i) {
    console.assert(A[i] == A_copy[i])
}
A[0] = 'a totally new value'
console.assert(A[0] != A_copy[0])
```

The array equality test could go badly for lists of different lengths.

```node {cmd}
function array_eq_simple(a, b) {
    for (i = 0; i < a.length; ++ i) {
        if (a[i] != b[i]) {
            return false
        }
    }
    return true
}

a = [1,2,3,4]
b = [1,2,3,4,5]
console.log(array_eq_simple(a,b)) // OOPS!
console.log(array_eq_simple(b,a))  // No index error!?
```

### Objects

We will discuss object-oriented programming in JS in a later section.
For now, we will talk about the `Object` type in JS.
This is an associative array that pythonistas can think of as a dictionary with keys that are strings.

```python {cmd}
D = {}
D['one'] = 1
D['two'] = 2
print(D)
```

```node {cmd}
D = new Object()
D['one'] = 1
D['two'] = 2
console.log(D)
```

Notice that the JS `Object` prints almost identically to the Python `dict`.
The main difference is that there is no need for quotes because it is assumed that all keys are strings.
For both languages, these print outs can also be used to define literals.

```python {cmd}
D = {'one': 1, 'two': 2}
print(D)
```

Note that the object literal doesn't require the `new` keyword.

```node {cmd}
D = {one: 1, two: 2};
D[5] = 'Automatic String Conversion?!';
console.log(D);
```

Naturally, the JS object is not as versatile as a dictionary, but the requirement of having strings for keys allows us to use **dot notation** to access the items.
In fact, this would be the normal way to access items.
The following does not work in Python.

```node {cmd}
myobject = {one: 1, two: 2, a_string: "Hello, World!"}
console.log(myobject.one)
console.log(myobject.two)
console.log(myobject.a_string)
```

Assignment can also use the dot notation.
```node {cmd}
myobject = {}
myobject.one = 1
console.log(myobject.one)
```



### Some other types

Boolean, ArrayBuffer, Map

## Conditional Statements

Let's consider a basic `if`/`else` statement in Python.

```python {cmd}
x = 4
if x < 5:
    print(x, "is less than 5")
else:
    print(x, "is at least 5")

x = 7
if x < 5:
    print(x, "is less than 5")
else:
    print(x, "is at least 5")
```

Here is a similar construction in JS.

```node {cmd}
let x = 4
if (x < 5) {
    console.log(x + " is less than 5")  
} else {
    console.log(x + " is at least 5")
}

x = 7
if (x < 5) {
    console.log(x + " is less than 5")  
} else {
    console.log(x + " is at least 5")
}
```

The important differences to note are the following.
1. The predicate `x < 5` must be in parentheses.
2. Curly braces delimit the block.

In both languages, the `else` clause is optional.

### Ternary Operators

Both Python and JS support a ternary operator.
This is an expression is composed of a boolean expression `b`, and two other expressions `e_true` and `e_false`.  If the boolean evaluates to true, then the whole expression evaluates to `e_true`; otherwise it evaluates to `e_false`.  In short, it looks like the following.

**In Python:** `e_true if b else e_false`

**In JS:** `b ? e_true : e_false`

Here is a real example in Python.
Note that it uses the keywords `if` and `else` in a way that is different from a normal conditional statement.
Python detects that they are in the middle of an expression in order to treat them differently.

```python {cmd}
x = 4
print(x, 'is less than five' if x < 5 else 'is at least five')
x = 7
print(x, 'is less than five' if x < 5 else 'is at least five')
```

The ordering of the expressions is different for the JS example.
The boolean expression is given first.
It is followed by a question mark (`?`), then the expression for the true case, and finally, a colon (`:`) and the expression for the false case.

```node {cmd}
let x = 4;
console.log(x + ((x < 5) ? ' is less than five' : 'is at least five'));
let x = 7;
console.log(x + ((x < 5) ? ' is less than five' : ' is at least five'));
```

One difficulty in using the ternary operator is that it can lead to long lines of code that are tricky for a human to parse.
In general, one must think carefully about whether the gain of a few lines of code is worth the cost in readability.

It is important to remember that the entire ternary expression is a single expression.
If you try to use it as part of a larger expression, it is wise to parenthesize it.
For example, if one leaves out the parentheses around the ternary expression in the example above, it changes the behavior.
Can you see why?

## Functions

Here is a function defined in Python.
```python {cmd}
def f(x, y):
    return x + y

print(f(2,3))
```

Here is an equivalent function defined in JavaScript.
```node {cmd}
function f(x, y) {
    return x + y;
}

console.log(f(2,3))
```

The cosmetic syntactic differences are
1. The use of `function` instead of `def`.
2. The use of curly braces rather than tabs to delimit the code block.
3. Semicolons to end lines.

The similarities are comforting.
Both languages use the `return` keyword.

### Variable Scope in Functions

Functions are first-class objects in JavaScript, just as in Python.
Therefore, you can think of function definitions as defining a new object.
As objects, they also define a namespace and therefore variables are scoped into the namespace of the function.

```python {cmd}
x = 7
z = 1000
def f(x, y):
    return x + y + z

def g(x, y):
    z = 0
    return x + y + z

print(f(2,3))
print(g(2,3))
```

Here is an equivalent function defined in JavaScript.
```node {cmd}
let x = 7
const z = 1000
function f(x, y) {
    return x + y + z;
}

function g(x, y) {
    let z = 0;
    return x + y + z;
}

console.log(f(2,3))
console.log(g(2,3))
```

One way that the scoping rules behave differently is when one assigns to a variable from outside the scope.
In Python, a new variable in the scope is created.
In JavaScript, the assignment happens.

```python {cmd}
x = 'global'
def f():
    x = 'local'

f()
print(x)
```

```node {cmd}
let x = 'global';
function f() {
    x = 'local';
}

f();
console.log(x);
```

I suspect this would be a common mistake by programmers used to Python.
To get the equivalent behavior in JS, one would have to redeclare the variable inside the function using `var`, `let`, or `const`.

```node {cmd}
let x = 'global';
function f() {
    let x = 'local';
}

f(x);
console.log(x);
```

Note that this also works if the global variable is a constant.

```node {cmd}
const x = 100;
function f() {
    let x = 1000;
}

f(x);
console.log(x);
```

It makes sense because we are not overwriting the variable.
This is one motivation for properly marking variables that should be constant.
It ensures that we get an error if we forget to declare a variable in a lower scope and accidentally overwrite some global variable.
This is demonstrated below.

```node {cmd}
const x = 'global';
function f() {
    x = 'local';
}

f();
console.log(x);
```

Parameters are automatically defined as new local variables, so the following does just what you would hope it does.

```node {cmd}
const x = 'global';
function f(x) {
    x = 'local';
}

f(x);
console.log(x);
```

All of these are examples of the normal scoping rules that apply throughout JS.
There is nothing special about functions except the automatic definition of local variables to store parameters.

## Loops

### `while` loops

The simplest kind of loop is a `while` loop.
As long as a certain condition is true, the loop will continue.
Both Python and JS have `while` loops and both use the same keyword.

```python {cmd}
x = 0
while x < 40:
    print(x)
    x = x + 10
```

```node {cmd}
let x = 0;
while (x < 40) {
    console.log(x);
    x = x + 10;
}
```

Just as with conditional statements, you will need to put the boolean expression (i.e. the loop condition) in parentheses.


### `for` loops

Let's loop four times in Python.

```python {cmd}
i = 100
for i in range(4):
    print(i)
print("After the loop, i =", i)
```

In JS, we would do the following.

```node {cmd}
let i = 100;
for (i = 0; i < 4; i = i + 1) {
    console.log(i);
}
console.log("After the loop, i = " + i)
```

This may look quite strange if you are coming from Python.
If you're coming from C/C++, you recognize this syntax immediately.
The semicolons in the parenthesized `for` statement are a hint that we have three different statements.
The first is the statement that initializes the loop (`i= 0`).
The second (`i < 4`) is the boolean expression that must evaluate to true in order for the loop to do another iteration.
The third (`i = i + 1`) is the update that happens after each iteration.

The other difference that is highlighted by this example is that the update will execute until the loop condition is false.
So, the loop completes with `i` equal to `4`.
This is different from the Python case, which is simply iterating over the `range` object.
Thus, the Python loop ends with `i` equal to `3`.

You would almost never see code like the above.
The statement `i = i + 1` is usually written in its equivalent and shorter form: `i++` or `++i`.
So, the loop would be written as follows.

```node {cmd}
for (i = 0; i < 4; ++i) {
    console.log(i)
}
```

Just as in Python, it's possible to have a for loop that iterates zero times because the loop condition is checked at the **top** of each iteration.

```node {cmd}
for (i = 1; i < 1; ++i) {
    console.log("This is not going to print.")
}
console.log("The loop is done.")
```
### Iterating over collections

In Python, many loops are just iterations over a collection.
The `for` loop above is just an example of this; it is just iterating over the `range` object.

```python {cmd}
L = ['a', 'list', 'of strings']
for item in L:
    print(item)
```

Looping over the items in an iterable collection also works in JS.

```node {cmd}
let A = ['a', 'list', 'of strings'];
for (item of A) {
  console.log(item)
}
```

Be careful, to use `of` and not `in` here.
It is also possible to iterate with `for (item in iterable)` but it does something different.
Check it out.

```node {cmd}
let A = ['a', 'list', 'of strings'];
A.foo = 'some other thing that I attached to the array.'
for (item in A) {
  console.log(item)
}
```

It is iterating over the "names" of the items in `A`.
This is a little like iterating over the keys of a dictionary in Python.

Often, when iterating over a collection, it helps to have direct access to both the items **and** their indices.
In Python, this is done with `enumerate`.

```python {cmd}
L = ['a', 'list', 'of strings']
for index, item in enumerate(L):
    print(index, item)
```

Okay, brace yourself.  Here's the javascript.

```node {cmd}
let A = ['a', 'list', 'of strings'];
A.forEach(function(item, index) {
    console.log(index, item);
})
```

What just happened?
We defined a new function inside the parentheses of the `forEach` function.
This is the accepted syntax, but it might make a little more sense to see it written a little differently.

```node {cmd}
let L = ['a', 'list', 'of strings'];
function printit(item, index) {
    console.log(index, item);
}

L.forEach(printit)
```

That is, we just pass the function that we would like to be called on each item.

Looking at the above code, you might think it could be equivalent to the following.

```node {cmd}
let L = ['a', 'list', 'of strings'];
L.forEach(console.log)
```

It's not the same!
Notice that console.logging the items directly printed the item, the index, and then the list itself.
That is a convenience that JS provides.
The function that defines the loop body should actually take all three parameters.

```node {cmd}
let L = ['a', 'list', 'of strings'];
function printit(item, index, array) {
    console.log(index, item, array);
}

L.forEach(printit)
```

## Handling Errors

In Python, the principal error handling mechanism is the `Exception`.


## String Processing

## Working with Files

### Read from a File

### Write to a File
