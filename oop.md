# Object-Oriented Programming

## Classes

Classes in Javascript are defined primarily by their constructor.
One can think of a constructor as playing the role of an initializer (`__init__`) in Python.
It is a function that is executed after a new object is created to initialize variables, etc.

Here is a class definition in Python and the equivalent class in JS.

```python {cmd}
class Foo:
    def __init__(self, x):
        self.x = x
        self.zero = 0

f = Foo(5)
print(f.x, f.zero)
```

```node {cmd}
function Foo(x) {
    this.x = x;
    this.zero = 0;
}

var f = new Foo(5);
console.log(f)
```

The keyword `this` is used inside a constructor similar to the `self` variable in Python.
However, `this` does not need to be explicitly declared as a parameter to the constructor.

Instantiating objects is similar, with one important difference.
The `new` keyword tells JS to treat this function call as a constructor.
This means it will create a new object, assign it type `Foo`, assign the new object to the variable `this`.
Look what happens when we leave it out.

```node {cmd}
function Foo(x) {
    this.x = x;
    this.zero = 0;
}

var f = Foo(5);
console.log(f)
```

The function runs, but it doesn't seem to return a new object as desired.
You might wonder what happened to the `this.x` and `this.y` assignments.
If the new object didn't get created, you might expect these statements to give an error.
The answer is a little funny.
You might get a hint if you run the following code.
Suffice it to say, that `this` will always be some object, but maybe not the one you expect.

```node {cmd output="none"}
function Foo(x) {
    this.x = x;
    this.zero = 0;
    console.log(this)
}

var f = new Foo(5);
var g = Foo(5);
```

Just as with `__init__`, the constructor is an ordinary function.
It requires some hint from the code to do object construction when it is called.
In Python, you don't (usually) call `__init__` manually (except for some inheritance situations).
Instead, you treat the class as a function.
In JS, the `new` keyword provides this hint.
Python programmers will forget to include it when first acclimating to JS.

### Methods

Adding a method to a class is exactly like adding any another attribute (or, property in the JS lingo).

In Python, one of the reasons for explicitly including the instance in the parameters of a method is to make a distinction between a `function` and a `method`.

If you haven't seen this distinction before, consider the following Python code.

```python {cmd}
class Foo:
    def mymethod(self):
        print("You called my method.")

f = Foo()
print(str(type(f.mymethod))[1:-1])
print(str(type(Foo.mymethod))[1:-1])
```

The same function seems to be in two different namespaces.
First, it's in the namespace of the object, and second, it's in the namespace of the class.
So, where is it really?
The answer in this case is the class.

Python and JS both treat function calls as a kind of message passing.
In calling a method, you are asking that object to find the right function and call it.
In Python, the search for the right function is a dictionary lookup.
If the desired function is not in the dictionary for the object, the search progresses to the class, and then to the superclass(es) if necessary.
The order of the search is called the **method resolution order** (**MRO**).
The magic transformation from a function to a method happens after the function is not found on the object itself.

It is possible to attach a function directly to an object as in the following.

```python {cmd}
def myfunction():
    print("You called my function.")

class Foo:
    pass

f = Foo()
f.not_a_method = myfunction

print(str(type(f.not_a_method))[1:-1])
f.not_a_method()
print(str(type(Foo.not_a_method))[1:-1])
```

Something similar is possible in JS.
Here is an example.

```node {cmd}
function myfunction() {
    console.log("You called my function.")
}

function Foo() {
    this.like_a_method = myfunction;
}

f = new Foo()
console.log(typeof(f.like_a_method))
f.like_a_method()
console.log(typeof(Foo.like_a_method))
```
