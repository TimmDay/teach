# var, let and const in the zoo
### declaring variables in javascript ES6
TLDR;

> Use const by default, change to let when you need to be able to re-assign values to the variable. Avoid var altogether.

That's my plan at least.

Let's break down the differences between var, let and const in detail.

|Summary of Differences| var | let | const |
|--|--|--|--|
|re-declarable |Y|N|N|
|re-assignable|Y|Y|N|
|function scoped|Y|Y|Y|
|block scoped|N|Y|Y|
|hoisting|Y|N|N|

So what does that mean?

In 10 minutes when we are done you will have a thorough understanding of what is going on with var, let and const. We will discuss the terms in this table one by one. First, some definitions...

To **declare** a variable without initializing looks like this:

``var a;``

To declare and **initialize** a variable looks like this:

```
var a = 1;
let b = 2;
const c = 3;
```

To **reassign** a variable after it has been initialized looks like this

```
a = 10;
b = 20;
c = 30; //error - read on to find out why
```

Also notice that there is only one difference between const and let. Const has the restriction that it is not re-assignable (no rebinding is allowed). Otherwise *let* and *const* act the same.

## Re-declarable

Variables that are declared with var are re-declarable, those declared with let and const are not. This is a bad thing for var.

| | var | let | const |
|------|-----|-----|----|
|re-declarable |Y|N|N|

For example

```
let lions = 3;
let lions = 200; //ERROR code will not run
```

```
var lions = 3;
var lions = 200; //code runs
```

With both *var* and *let* we can reassign values to variables as normal. But let does NOT allow you to re-declare another variable with a name that is already being used. Why would we ever want to do that anyway?
Truth is, most re-declarations are accidents and lead to problems. Consider this.

A zoo has a big clunky program that tracks the number of animals. Near the start of the program there is a line:

``var count = 3; //number of lions``

A couple of months later, the zoo opens a new penguin enclosure, and an intern tacks this to the end of the program:

``var count = 200; //number of penguins``

You can see the problem. The variable name *count* has been accidentally overwritten, and because *var* was used the error was not flagged.

Count now means a totally new thing, but parts of the program might expect it to still mean the old thing. Any part of the program that expects count to represent the number of lions is now corrupted with incorrect information. It is the lion apocalypse.

A function that calculated the amount of meat to buy to feed the lions, is now calculating the cost of lunch for 200 lions! The three actual lions are either going to get really fat, or be released into the suburbs when the zoo goes bankrupt. People will get eaten by lions if you use \*var\*.

Javascript throws no errors if you make this error with *var*. With *let* or *const*, however, your program will flag a problem and the bug will not be introduced.

This alone is a good reason to drop all var use.

## Re-assignable

Const is not re-assignable.

|| var | let | const |
|--|--|--|--|
|re-assignable|Y|Y|N|

For example, after a lion escaped because the intern used var:

```
let lions = 3;
lions = 2;
// code runs
```

```
const DAYS_IN_WEEK= 7;
DAYS_IN_WEEK = 6;
//ERROR code will not run
```
> if you are sure that the value of a constant variable will not change, it is conventional to write it in all uppercase with underscores for spaces
Why does this property make const useful?

- There is a small performance increase in some cases. Whenever your js is compiled for performance benefits (by something like babel), the const keyword lets the compiler know that it can treat this variable in a faster way. So why not use it where you can?

- It makes your code more readable. Seeing the \*const\* keyword tells the reader that this variable contains information that is not expected to change. Seeing the \*let\* keyword suggests that this variable will change. That is good to know when you are trying to understand code!

#### quick note on const - correcting a common misconception

>As we can see const cannot be reassigned, but it's value is NOT immutable (immutable means cannot be changed). It is the variable binding that is immutable - you cannot change what the variable name is sticking to. This is relevant when it comes to objects - we cannot change the object that the variable refers to, but we can change values within the object.

```
const x = 5;
x = 4; //ERROR - this is a reassignment of the variable x
```

```
const d = {
name: "g",
species: "dog"
}
d.name = "greg"; //fine. code runs
```

Again, with const it is the binding must remain constant. Not the value.

This also means that this:

``const pig;``

will bind to *undefined*, which is utterly pointless, so you can expect an 'initialised to undefined' error. Simply update to *let* if you find yourself in this scenario.

## Function scoped
All of var, let and const are function-scoped.

|| var | let | const |
|--|--|--|--|
|function scoped|Y|Y|Y|

This means that if a var, let or const variable is declared within a function, it will not be recognized outside of the function.

```
const fun = () => {
var giraffes = 10;
let bears = 11;
const hippos = 12;
}
console.log(bears); //ReferenceError: bears is not defined
```

None of the animals can escape from the function enclosure.

## Block scoped
let and const have the added bonus of being block-scoped.

|| var | let | const |
|--|--|--|--|
|block scoped|N|Y|Y|

```
if(true) {
var lions = 2;
let kangaroos = 9;
}

console.log(lions); // code runs
console.log(kangaroos); //ReferenceError: kangaroos is not defined
```

The lions have escaped from the block-enclosure. Thanks *var*.

If we are declaring a variable within a code block, we plan on using that variable within that block. Why should we let it out so that the variable name pollutes the namespace? We shouldn't. Using *let* and *const* protect against this.

## Hoisting

Just trust me that this is another good reason to use const and let.

|| var | let | const |
|--|--|--|--|
|hoisting|Y|N|N|

Hoisting is a funny javascript quirk with var that allows a variable to be used before it is declared.

This code runs without error.
```
cats = 14;
console.log(\`she has ${cats} cats\`);
var cats;
```

The var declared variable is 'hoisted' to the global scope.
\> does that string seem odd, with the ${} part within the back-ticks? This is a ES6 template string

Hoisting can be fun but it leads to bugs. Use let and const to avoid it and never worry about it again. Look up 'hoisting in javascript' if you want to know more.

## Conclusion

It seems to me that the best practice is:

- when declaring a new variable, use const by default.
- if you need to re-assign that variable, update the const to let.

We get all of the good bit of var, with the added benefits of more specific scope, less errors and more maintainability.

If you are writing code for an application that requires ecma5, and you therefore feel trapped with var, consider looking into \[babel\]([https://babeljs.io/](https://babeljs.io/)).

## Bonus Challenge

What prints to the console when you run this code?

```
const j = 34;
for(let j=0; j<4; j++){
console.log(j)
}
console.log(j)
```

I hope this helped clear up the differences between var, let and const and gave some extra info to those who are interested. Thanks for reading :)
