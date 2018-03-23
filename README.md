# OBJECT ORIENTED JAVASCRIPT

## Lesson 1: Objects in Depth

### 1.1 Create and Modify Properties
#### Creating Objects
Use object literal notation or the object() constructor function to create an object.
The object() method is slower and verbose hence the object literal is the recommended
way to create an object.
```javascript
// Using literal notation:
const myObject = {};
// Using the object constructor function:
const myObject = new Object();
```
#### Modifying Properties
Data within an object are *mutable*. Consider the ```cat``` object:
```javascript
const cat = {
 age: 2,
 name: 'Bailey',
 meow: function () {
   console.log('Meow!');
 },
 greet: function (name) {
   console.log(`Hello ${name}`);
 }
};
```
Let's modify the data a bit!
```javascript
cat.age += 1;
cat.age;
// 3
cat.name = 'Bambi';
cat.name;
// Bambi
```
Our ```cat``` now looks like this:
```javascript
const cat = {
 age: 3,
 name: 'Bambi',
 meow: function () {
   console.log('Meow!');
 },
 greet: function (name) {
   console.log(`Hello ${name}`);
 }
};
```
#### Adding Properties
To add a property, specify the property name and just give it a
value. You can either use dot notation or square brackets to add
properties. We can add a method to an object using the dot notation.
```javascript
const printer = {};

// Use dot notation:
printer.on = true;
printer.mode = 'black and white';

// Use square brackets:
printer['remainingSheets'] = 120;

// Add a method:
printer.print = function () {
  console.log('The printer is printing!');
};
```
Now our ```printer``` object looks like this:
```javascript
{
  on: true,
  mode: 'black and white',
  remainingSheets: 120,
  print: function () {
    console.log('The printer is printing!');
  }
}
```
#### Removing Properties
Since Objects are *mutable* you can not only add/modify properties, but also *delete* them.
We can delete properties from the ```printer``` objects as shown below:
```javascript
delete printer.mod;
// true
printer.mod;
// undefined
```
#### Passing Arguments
###### Passing a Primitive
In JavaScript, a primitive (e.g., a string, number, boolean, etc.) is immutable. In other words, any changes made to an argument inside a function effectively creates a copy local to that function, and does not affect the primitive outside of that function. Check out the following example:
```javascript
function changeToEight(n) {
  n = 8; // whatever n was, it is now 8... but only in this function!
}

let n = 7;

changeToEight(n);

console.log(n);
// 7
```
###### Passing an Object
On the other hand, objects in JavaScript are mutable. If you pass an object into a function, Javascript passes a reference to that object. Let's see what happens if we pass an object into a function and then modify a property:
```javascript
let originalObject = {
  favoriteColor: 'red'
};

function setToBlue(object) {
  object.favoriteColor = 'blue';
}

setToBlue(originalObject);

originalObject.favoriteColor;
// 'blue'
```
The reason why this is the case is because objects in JavaScript are passed by reference. If we make changes to the reference, we're actually directly modifying the original object itself. Therefore, when reassigning an object to a new variable, changing that copy chnages the original object as well.
```javascript
const iceCreamOriginal = {
  Andrew: 3,
  Richard: 15
};

const iceCreamCopy = iceCreamOriginal;

iceCreamCopy.Richard;
// 15
iceCreamCopy.Richard = 99;

iceCreamCopy.Richard;
// 99

iceCreamOriginal.Richard;
// 99
```
###### Comparing an Object with Another Object
On the topic of references, let's see what happens when we compare one object with another object. The following objects, parrot and pigeon, have the same methods and properties:
```javascript
const parrot = {
  group: 'bird',
  feathers: true,
  chirp: function () {
    console.log('Chirp chirp!');
  }
};

const pigeon = {
  group: 'bird',
  feathers: true,
  chirp: function () {
    console.log('Chirp chirp!');
  }
};

parrot == pigeon;
// false
```
The expression will only be true when comparing two references to exactly the same object.
```javascript
const myBird = parrot;
myBird == parrot;
// true
```
### 1.2 Invoking Object methods
#### Functions vs. methods
We can extend functionality to objects by adding methods to them.
```javascript
// Function
function sayHello () {
  console.log("Hi there!");
}

const developer = {
  name: 'Andrew'
};
```
If we want to add the sayHello() function into the ```developer``` object, we can add the same way as we add other new properties: by providing property name, then giving it a value. This time, the value of the property is a function!
```javascript
developer.sayHello = function () {
  console.log('Hi there!');
};
```
This is how the ```developer``` object looks like:
```javascript
{
  name: 'Andrew',
  sayHello: function () {
    console.log('Hi there!');
  }
}
```
#### Calling Methods
We can access a function in an object using the property name. Again, another name for a function property of an object is a method. We can access it the same way that we do with other properties: by using dot notation or square bracket notation. Let's take a look back at the updated developer object above, then invoke its `sayHello()` method:
```javascript
developer.sayHello();
// 'Hi there!'

developer['sayHello']();
// 'Hi there!
```
#### Passing Arguments Into methods
If the method takes arguments, you can proceed the same way, too:
```javascript
const developer = {
  name: 'Andrew',
  sayHello: function () {
    console.log('Hi there!');
  },
  favoriteLanguage: function (language) {
    console.log(`My favorite programming language is ${language}`);
  }
};


developer.favoriteLanguage('JavaScript');
// My favorite programming language is JavaScript'
```
#### Calling Methods by Property name
We've been using anonymous functions (i.e., functions without a name) for object methods. However, naming those functions is still valid JavaScript syntax. Consider the following object, ```greeter```:
```javascript
const greeter = {
  greet: function sayHello() {
    console.log('Hello!');
  }
};

greeter.greet();
// 'Hello!'
```
Named functions are great for a smoother debugging experience, since those functions will have a useful name to display in stack traces. They're completely optional, however, and you'll often read code written by developers who prefer one way or the other.

#### A Method Can Access the Object it was Called On
Using this, methods can directly access the object that it is called on. Consider the following object, `triangle`:
```javascript
const triangle = {
  type: 'scalene',
  identify: function () {
    console.log(`This is a ${this.type} triangle.`);
  }
};

triangle.identify();

// 'This is a scalene triangle.
```
### 1.3 Beware of Globals
#### ```this``` and Function Invocation
Let's compare the code from the chameleon object with the ```whoThis()``` code.
```javascript
const chameleon = {
  eyes: 2,
  lookAround: function () {
     console.log(`I see you with my ${this.eyes} eyes!`);
  }
};

chameleon.lookAround();
// I see you with my 2 eyes!

function whoThis () {
  this.trickyish = true
};

whoThis();
```
#### ```this``` in the Function/ Method
```JavaScript
// from the chameleon code:
console.log(`I see you with my ${this.eyes} eyes!`);

// from the whoThis() code:
this.trickyish = true
```
For our purposes of discovering the value of this, it does not matter that in the chameleon code, we're using this to retrieve a property, while in the whoThis() code, we're using this to set a property. So, in both of these cases, the use of this is virtually identical.

#### Compares the Structures of the Function/ Method
The lookAround() code is a method because it belongs to an object. Since it's a method, it's invoked as a property on the chameleon object:
```JavaScript
chameleon.lookAround();
```
`whoThis()` is not a method; it's a plain, old, regular function. And look at how the `whoThis()` function is invoked:
```javascript
whoThis();
```

#### ```this``` and Invocation
How the function is invoked determines the value of `this` inside the function.

Because `.lookAround()` is invoked as a method, the value of this inside of `.lookAround()` is whatever is left of the dot at invocation.

When a regular function is invoked, the value of `this` is the global window object.

#### The `window` Object
This window object has access to a ton of information about the page itself, including:
- The page's URL (`window.location;`)
- The vertical scroll position of the page (`window.scrollY'`)
- Scrolling to a new location (`window.scroll(0, window.scrollY + 200`); to scroll 200 pixels down from the current location)
- Opening a new web page (`window.open("https://www.udacity.com/");`)

#### Global Variables are Properties on `window`
Since the `window` object is at the highest (i.e., global) level, an interesting thing happens with global variable declarations. Every variable declaration that is made at the global level (outside of a function) automatically becomes a property on the `window` object!
```javascript
var currentlyEating = 'ice cream';

window.currentlyEating === currentlyEating
// true
```
###### Globals and `var`, `let`, and `const`
Only declaring variables with the `var` keyword will add them to the `window` object. If you declare a variable outside of a function with either `let` or `const`, it will not be added as a property to the `window` object.
```javascript
let currentlyEating = 'ice cream';

window.currentlyEating === currentlyEating
// false!
```
#### Global Functions are Methods on `window`
Similarly to how global variables are accessible as properties on the `window` object, any global function declarations are accessible on the `window` object as methods:
```javascript
function learnSomethingNew() {
  window.open('https://www.udacity.com/');
}

window.learnSomethingNew === learnSomethingNew
// true
```
#### Avoid Globals
There are a number of reasons why global variables are bot ideal:
- **Tight coupling** - Code is too dependent on the details of each other
- **Name collisions** - Occurs when two (or more) functions depend on a variable with the same name. A major problem with this is that both functions will try to update the variable and or set the variable, but these changes are overridden by each other.

```javascript
let counter = 1;

function addDivToHeader () {
  const newDiv = document.createElement('div');
  newDiv.textContent = 'div number ' + counter;

  counter = counter + 1;

  const headerSection = document.querySelector('header');
  headerSection.appendChild(newDiv)
}

function addDivToFooter() {
  const newDiv = document.createElement('div');
  newDiv.textContent = 'div number ' + counter;

  counter = counter + 1;

  const headerSection = document.querySelector('footer');
  headerSection.appendChild(newDiv)
}
```
### 1.4 Extracting Properties and Values
#### Object methods
```javascript
const myNewFancyObject = new Object();
```
The `Object()` function actually includes a few methods of its own to aid in the development of your applications. These methods are:
- `Object.keys()`
- `Object.values()`
#### Object.keys() and Object.values()

```javascript
const dictionary = {
  car: 'automobile',
  apple: 'healthy snack',
  cat: 'cute furry animal',
  dog: 'best friend'
};

Object.keys(dictionary);

// [car, apple, 'cat', 'dog']

Object.values(dictionary);

// ['automobile', 'healthy snack', 'cute furry animal', 'best friend']
```
The methods return the keys and values of an object respectively, in an array.
## Lesson 2: Functions at Runtime
### 2.1 First-Class Functions
n JavaScript, functions are first-class functions. This means that you can do with a function just about anything that you can do with other elements, such as numbers, strings, objects, arrays, etc. JavaScript functions can:
 - Be stored in variables
 - Be returned from a function.
 - Be passed as arguments into another function.

#### Functions can Return functions

```javascript
function alertThenReturn() {
  alert('Message 1!');

  return function () {
    alert('Message 2!');
  };
}
```
If `alertThenReturn()` is invoked in a browser, we'll first see an alert message that says `'Message 1!'`, followed by the `alertThenReturn()` function returning an anonymous function. However, we don't actually see an alert that says `'Message 2!'`, since none of the code from the inner function is executed. How do we go about executing the returned function?

Since `alertThenReturn()` returns that inner function, we can assign a variable to that return value:
```javascript
const innerFunction = alertThenReturn();

innerFunction();

// alerts 'Message 2!'
```
The `innerFunction` is used like any other function.

Likewise, this function can be invoked immediately without being stored in a variable. We'll still get the same outcome if we simply add another set of parentheses to the expression `alertThenReturn();`:
```javascript
alertThenReturn()();

// alerts 'Message 1!' then alerts 'Message 2!'
```
### 2.2 Callbacks
#### Callback functions
A function that takes other functions as arguments (and/or returns a function) is known as a **higher-order function**. A function that is passed as an argument into another function is called a **callback function**.

```javascript
function callAndAdd(n, callbackFunction){
  return n + callbackFunction();
}

function returnsThree(){
  return 3;
}

callAndAdd(2, returnsThree);
// 5
```
#### Array Methods
- `forEach()`
- `map()`
- `filter`

###### forEach()
Array's `forEach()` method takes in a callback function and invokes that function for each element in the array. In other words, `forEach()` allows you to iterate (i.e., loop) through an array, similar to using a `for` loop. Check out its signature:
```javascript
array.forEach(function callback(currentValue, index, array) {
    // function code here
});
```
The callback function itself receives the arguments: the current array element, its index, and the entire array itself.

```javascript
[1,5,2,4,6,3].forEach(function logIfOdd(n) {
  if (n % 2 !== 0){
    console.log(n)
  }
});
```
###### map()
Array's `map()` method is similar to `forEach()` in that it invokes a callback function for each element in an array. However, `map()` returns a new array based on what's returned from the callback function. Check out the following:
```javascript
const names = ['David', 'Richard', 'Veronika'];

const nameLengths = names.map(function(name) {
  return name.length;
});
```
### 2.3 Scope
There is block and function scope. These determine where a variable can be seen in some code. There is also **runtime scope**. This scope represents the *context* of the function, specially, the set of variables available for the function to use.

The code inside a function has access to:
- The function's arguments.
- Local variables declared within the function.
- Variables from its parent function's Scope.
- Global variables.

```javascript
const myName = 'Andrew';
// Global variable

function introduceMyself() {

  const you = 'student';
  // Variable declared where introduce() is defined
  // (i.e., within introduce()'s parent function, introduceMyself())

  function introduce() {
    console.log(`Hello, ${you}, I'm ${myName}!`);
  }

  return introduce();
}
```
##### Scope Chaining
When it comes to accessing variables, the JavaScript engine will traverse the scope chain, first looking at the innermost level (e.g., a function's local variables), then to outer scopes, eventually reaching the global scope if necessary.

##### Variable Shadowing
What happens when you create a variable with the same name as another variable somewhere in the scope chain?

JavaScript won't throw an error or otherwise prevent you from creating that extra variable. In fact, the variable with local scope will just temporarily "shadow" the variable in the outer scope. This is called variable shadowing. Consider the following example:
```javascript
const symbol = '¥';

function displayPrice(price) {
  const symbol = '$';
  console.log(symbol + price);
}

displayPrice('80');
// '$80'
```

### 2.4 Closures
A closure refers to the combination of a function and the lexical environment in which that function was declared. Every time a function is defined, closure is created for that function. This is especially especially powerful in situations where a function is defined within another function, allowing the nested function to access variables outside of it. Functions also keep a link to its parent's scope even if the parent has returned. This prevents data in its parents from being garbage collected.

### Immediately-Invoked Function Expressions (IIFE)
An immediately-invoked function expression, or IIFE (pronounced iffy), is a function that is called immediately after it is defined.
```javascript
(function sayHi(){
    alert('Hi there!');
  }
)();

// alerts 'Hi there!'
```
##### Passing Arguments into IIFE's
```javascript
(function (name){
    alert(`Hi, ${name}`);
  }
)('Andrew');

// alerts 'Hi, Andrew'
```

## Lesson 3: Classes and Objects
### 3.1 Constructor Functions
To instantiate (i.e., create) a new object, we use the new operator to invoke the function:
```javascript
new SoftwareDeveloper();
```
What does make `SoftwareDeveloper` a constructor function are:
- The use of the `new` operator to invoke the function
- How the function is coded internally (which we'll look at right now!)

##### Constructor Functions Structure and Syntax
```javascript
function SoftwareDeveloper() {
  this.favoriteLanguage = 'JavaScript';
}
```
First, rather than declaring local variables, constructor functions persist data with the `this` keyword. The above function will add a `favoriteLanguage` property to any object that it creates, and assigns it a default value of `'JavaScript'`. Don't worry too much about `this` in a constructor function for now; just know that `this` refers to the `new` object that was created by using the `new` keyword in front of the constructor function.

##### Creating a New Object
Let's use the `new` operator to create a new object:
