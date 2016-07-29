# Using "new" to create objects

The regular `{...}` syntax allows to create one object. But often we need to create many similar objects.

That can be done using constructor functions and the `"new"` operator.

[cut]

## Constructor function

Constructor functions technically are regular functions. There are two agreements though:

1. They are named with capital letter first.
2. They should be executed only with `"new"` operator.

For instance:

```js run
function User(name) {
  this.name = name;
  this.isAdmin = false;
}

*!*
let user = new User("Jack");
*/!*

alert(user.name); // Jack
alert(user.isAdmin); // false
```

When a function is executed as `new User(...)`, it does the following steps:

1. A new empty object is created and assigned to `this`.
2. The function executes. Usually it modifies `this`, adds new properties to it.
3. The value of `this` is returned.

In other words, `new User(...)` does something like:

```js
function User(name) {
*!*
  // this = {};  (implicitly)
*/!*

  // we add properties to this
  this.name = name;
  this.isAdmin = false;

*!*
  // return this;  (implicitly)
*/!*
}
```

So the result of `new User("Jack")` is the same object as:

```js
let user = {
  name: "Jack",
  isAdmin: false
};
```

Now if we want to create other users, we can call `new User("Ann")`, `new User("Alice")` and so on. Much shorter than using literals every time, and also reads well. 

That's the main purpose of constructors -- to implement reusable object creation code.

Let's note once again -- technically, any function can be used as a constructor. That is: any function can be run with `new`, and it will execute the algorithm above. The "capital letter first" is a common agreement, to make it clear that a function is to be run with `new`.

````smart header="new function() { ... }"
If we have many lines of code all about creation of a single complex object, we can wrap them in constructor function, like this:

```js
let user = new function() {
  this.name = "John";
  this.isAdmin = false;

  // ...other code for user creation
  // maybe complex logic and statements
  // local variables etc
};
```

The constructor can't be called again, because it is not saved anywhere, just created and called. So this trick aims to encapsulate the code for a single complex object only.
````

## Return from constructors

Usually, constructors do not have a `return` statement. Their task is to write all necessary stuff into `this`, that automatically becomes the result.

But if there is a `return` statement, then the rule is simple:

- If `return` is called with object, then it is returned instead of `this`.
- If `return` is called with a primitive, it's ignored.

In other words, `return` with an object returns that object, with a non-object value -- returns `this` as usual.

For instance, returning an object:

```js run 
function BigUser() {

  this.name = "John";

  return { name: "Godzilla" };  // <-- returns an object
}

alert( new BigUser().name );  // Godzilla, got that object
```

And here's an example with returning a string:

```js run 
function SmallUser() {

  this.name = "John";

  return "Mouse"; // <-- returns a primitive
}

alert( new SmallUser().name );  // John, got "this" (mouse disappeared)
```

Once again, most of time constructors return nothing. Here we mention this special aspect of constructors for completeness.

````smart header="Omitting brackets"
By the way, we can omit brackets after `new` with zero arguments:

```js
let user = new User; // <-- no brackets
// same as
let user = new User();
```

Omitting brackets here is not considered a "good style", but the syntax is allowed by specification.
````

## Methods in constructor

Using constuctor functions to create objects gives a great deal of flexibility. The constructor may have parameters that define how to construct the object, what to put in it.

Let's add a method.

For instance, `new User(name)` below creates an object with the given `name` and the method `sayHi`:

```js run
function User(name) {
  this.name = name;

  this.sayHi = function() {
    alert( "My name is: " + this.name );
  };
}

*!*
let john = new User("John");

john.sayHi(); // My name is: John
*/!*

/*
john = {
   name: "John",
   sayHi: function
}
*/
```

## Summary

In this chapter we studied the basics of object constructors. 

- Constructor functions are regular functions, but there's a common agreement to name them with capital letter first.
- Constructor functions should only be called using `new`. Such call implies a creation of empty `this` at the start and returning the populated one at the end.

We can already use constructor functions to make multiple similar objects. But the topic is much wider than described here. So we'll return it later and cover more in-depth.

Now it's important to understand what `new` is, because Javascript provides constructor functions for many built-in language objects: like `Date` for dates, `Set` for sets and others that we plan to study.


