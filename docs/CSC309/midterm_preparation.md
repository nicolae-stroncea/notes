# Midterm Preparation

> Always consider hoisting(both for functions AND variables(except ignore it IFF its block variables ))

## Scopes

Scopes determine the accessibility of variables inside a JS program. JS defines 3 kinds of scope:

1. **Functional Scope**(local variables. Functions declared inside function(with var, or with let if you declare it at the beginning), can only be accessed from there)

2. **Global Scope**

3. **Block Scope**

## Hoisting

### Variables Hoisting

* All **variable** and **function declarations** are 'hoisted' up to the <mark>top of their  function scope(or global scope)</mark>. Variable *definitions* stay in place

```javascript
console.log(a)
var a = 3
// is hoisted up as:
var a;
console.log(a);
a = 3;
```

```javascript
var name = "Baggins";

(function () {
    // Outputs: "Original name was undefined"
    console.log("Original name was " + name);

    var name = "Underhill";

    // Outputs: "New name is Underhill"
    console.log("New name is " + name);
})();
```

* What happens if you use `a = 3` instead of `var a = 3`.  In that case a  is considered a global varaible. **It now becomes avilable to everybody in the lexical scope, which is hard to manage.**

### Function Hoisting

> Inner functions(and inner variables) are hoisted to the top of the function they're part of, which is why they're local, you can't reference them outside of that function's scope.

* Function name + Definition gets hoisted to the top

```javascript
// Outputs: "Yes!"
isItHoisted();

function isItHoisted() {
    console.log("Yes!");
}
```

> Notice that we can call isItHoisted before hand

* <mark>Hoisting only occurs for function declarations not function expression. The assignment part of a function expression is NOT hoisted, only the declaration is(and if it's a function declaration, its body is taken as well.)</mark>.  so if you assign a function to a variable, it's not going to work. This is because function doesn't have a name, so the function is probably hoisted but you can't refer to it. variable definitionNotHoisted is also hoisted, but it is undefined at the top since it's not declared yet. 

```javascript
// Outputs: "Definition hoisted!"
definitionHoisted();

// TypeError: undefined is not a function
definitionNotHoisted();

function definitionHoisted() {
    console.log("Definition hoisted!");
}

var definitionNotHoisted = function () {
    console.log("Definition not hoisted!");
};
```

Here we give the function a name, test. **This one below still doesn't work, because it's part of a function expression, it's not a direct function assignment.**

```javascript
// Outputs: "Definition hoisted!"
definitionHoisted();

// TypeError: undefined is not a function
test();

function definitionHoisted() {
    console.log("Definition hoisted!");
}

var definitionNotHoisted = function test () {
    console.log("Definition not hoisted!");
};
```

### Class hoisting

* Class declarations are not hoisted. This is because they're actually function assignments.

```javascript
(function() {          //IIFE
    console.log(hero); //undefined
    var hero = "Batman";
    theFlash();        //TypeError: theFlash is not a function
    var theFlash = function(){
        console.log("Fastest Man alive");
    }
})();
```

This is interpreted as the below

```javascript
(function() {          //IIFE
    var hero;          //declarations are hoisted but NOT assignment
    var theFlash;
    console.log(hero); //undefined
    hero = "Batman";
    theFlash();        //TypeError: theFlash is not a function
    theFlash = function(){
        console.log("Fastest Man alive");
    }
})();
```

### Blocks

> This is what actually happens, but for all intents and purposes we can consider that block scope variables HAVE NO hoisting:
> 
>     In ECMAScript 2015, `let` and `const` keyword variable declarations will hoist the variable to top of the 
> block. However, referencing the variable in the block before variable 
> declaration results in `ReferenceError` . The difference between `var/function` declarations and `let/const` declarations is in the initialization portion. The former are initialized with `undefined` or generator function right when binding is created at top of the scope, while the latter stay uninitialized until `let/const` statement is run. The variable is called to be in a “temporal dead 
> zone” from start of the block until the declaration is executed.

* Minimize hoisting problems since we don't do hoisting anymore. Scope is block only(can only be used inside of the curly braces its declared in), and inside of the block you don't have hoisting, so you cannot use it before you declare it.

* A block is defined by a pair of curly braces

* Block variables(let and const) are not hoisted to the top of the block. <mark>You can only access block variables AFTER they have been declared</mark>. Until then, the variable is oconsidered to be in the "Temporal Dead Zone". If you try to access it, it will result in reference error.

* <font color="red">Both block and function(var) level variables are only available within their scope and **in inner scopes.**</font>

* If you declare them at a global level, they're not available within functions. Might be because they're not actually in any `{}`, so they're not in any block.

#### Const

* Needs to be declared immediately(b/c once you assign it a value, you cannot reassign it)

* You cannot reassign it to a different memory address, but you can change its properties.

When you declare a variable using *var* on the root level, it is automatically declared on the global object. This does not happen with let or const.

```javascript
var foo = 42;
console.log(window.foo) //42
```

When using *let* (and *const*), this does not happen:

```javascript
let foo = 42;
console.log(window.foo) //undefined
```

## Immediately Invoked Function Expression

```javascript
(function () {
    var foo = 42;
})();

console.log(foo); // ReferenceError: foo is not defined
```

## Lexical Scope

> Lexical Scope allows inner functions to access the scope of their outer functions.

---

## Functions

Functions in javascript are first-class objects. This is becauseJS functions are actually types of objects

> This means the language supports constructing new functions during the execution of a program, storing them in data structures, passing them as arguments to other functions, and returning them as values of other functions.

- They can be stored in a variable, object or array

- Can be passed as an argument to a function

- Returned from a function.

### Creating Functions

**Function Declaration**: 

```javascript
// Named function declaration
function myFunction (parameters) { /* logic here */ }
```

**Function Expression**:

```javascript
// Assignment of a function expression to a variable
var myFunction = function () { /* logic here */ };
var myFunction = function test() {/* statements */}
```

A function expression is similar to and has the same syntax as a function declaration (see function expression for details). A function expression may be a part of a larger expression. One can define "named" function expressions (where the name of the expression might be used in the call stack for example) or "anonymous" function expressions. Function expressions are not hoisted onto the beginning of the scope, therefore they cannot be used before they appear in the code.

```javascript
    myFunction: function () { /* logic here */ }
```

Function expression may be part of a larger expression. You can define **named**"function expressions or **anynonymous** function expressions. <font color="blue">Only the variable declaration is hoisted to the beginning of the scope. The function expresssion IS NOT.</font>

### Arrow function

Arrow function is an anonymous function(unless you assign it to a variable). [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), do not have their own `this`,  `this` retains the value of the enclosing lexical context's `this`(i.e binds **`this`** by itself). You also cannot override that with any calls to `bind()`, `call()` or `apply()`  In global code, `this`  will be set to the global object:

```javascript
(params)=>{statements}
```

* Cannot be used as constructors

> What ends up happening is that you call obj.bar(). That binds `this` to be the `obj`, because of **runtime binding**. The arrow function is permanently bound to the `this` of its enclosing function, therefore the moment you call `obj.bar()`, it arrow function `this` instantly becomes obj, since that's the `this` of the enclosing function.

```js

var obj = {
  bar: function() {
    var x = (() => this); //x is not a value, but a function(anonymous one)
    return x;
  }
};

// Call bar as a method of obj, setting its this to obj
// Assign a reference to the returned function to fn
var fn = obj.bar();

// Call fn without setting this, would normally default
// to the global object or undefined in strict mode
console.log(fn() === obj); // true

// But caution if you reference the method of obj without calling it. You haven't executed it yet, therefore this has not yet been bound
var fn2 = obj.bar;
// You only execute the function now, which binds the this to global since it's not bound to any other object. It follows the this from fn2.
console.log(fn2()() == window); // true
```

### Immediately Invoked Function Expressions

**IIFE**: You invoke it immediately after.

* Any variables declared within the IFFE cannot be accessed outside the world

* We used to use it before EC6 in order to obtain block-level variables(those variables would only be defined inside of the IIFE) and after are not accessible.

* This is why it's immediately invoked. We can consider it as a block. We don't give it a name, because it's a one time thing. 

```javascript
(function foo() {
    // logic here
})();
```

### Closures

> A **closure** is the combination of a function bundled together (enclosed) with references to its surrounding state (the **lexical environment**).
>  In other words, a closure gives you access to an outer function’s scope
>  from an inner function. <font color="blue">In JavaScript, closures are created every time a function is created, at function creation time.</font>
> 
> * To  use a closure, define a function inside another function and expose it.
>    To expose a function, return it or pass it to another function.
> 
> * The inner function will have access to the variables in the outer function scope, even after the outer function has returned.

* A **closure** is a function bundled together(**enclosed**) with references to its surrounding state(**lexical environment**). This is achieved by having the functio nbe an inner function(in which case it will have access to the variables inside of its parent function), and then returning it.

* ALlows function/block scopes to be **preserved** even once they finish executing.

* <mark>A closure gives you access to an outer function's scope from an inner function</mark>. <font color="blue">Nested functions have access to the scope "above" them.</font>

Example below always returns 1, this is beacuse the coutner is initialized to 0 every time.

```javascript
function add(){
    var counter = 0;
    function plus(){
        counter +=1;
    }
    plus()
}
```

Example below is a correctly implemented counter.

```javascript
var add = (
    function(){
        var counter = 0
        return function(){
            counter +=1;
            return counter}
     })()
```

This version also works

```javascript
function testPlus(){

var counter = 0;
function plus(){
    counter +=1
    return counter
}
return plus
}
// advantage of this version over previous one is that you can essentially create multiple counters
a = testPlus()
b = testPlus()
```

1. Inner functions preserve the scope of the outer functions, EVEN IF the outer functions finish. THEREFORE, we will define an IIFE(immediately invoked function expression) which will set up the scope and the variables, and will return an inner function. We can assign that to a variable, so we have the inner function now as a variable, and every time we call it, because it preserves the scope of the IIFE, it can modify it, even though the IIFE executed only once and finished executing.

2. <mark>This makes JavaScript have "private" variables that can be modified, and stored.</mark>

3. The counter is protected by the scope of the anonymous function, 
   and can only be changed using the add function.

## Objects

```javascript
const student = {name:'Jimmy', year:2}
// is the same as
const student = {"name":'Jimmy', "year":2}

// both of them get generated as
// Object { name: "Jimmy", year: 2 }

// objec tproperties are retrieved like
student.name == student['name']
// can set those properties as
student.year = 20
// function can be stored as a value, we can put them into objects
student.sayName = function(){
    console.log("Hello" + this.name)    
}
student.sayName()
```

### `this` keyword

Value of `this ` is determined by **how** a function is called(runtime binding: `this` is bound at runtime(during runtime)).

* <font color="red">this becomes whatever you call it on( in`student.execfunction()` this is student, if you don't call the function on any object, this is global.</font> <mark>Starting with ES5, bind() can be used to set value of function's `this` regardless of how it's called.</mark> > Calling `f.bind(someObject)` creates a new function with the same body and scope as `f`, but where `this` occurs in the original function, in the new function it is permanently bound to the first argument of `bind`, regardless of how the function is being used.

* Now you can use `call()`, `apply()` and `bind()` to set the value of `this` to a particular value INDEPENDENT of the context it's in. So it doesn't matter what object you call it on, you override that with one of those 3 things.`execFunc.bind(obj)`

* global `this`: In web browsers, the window object is also the global object:), or global object if you're in node console. If we do `use strict` `this` becomes undefined when we're in an execution context(say inside of a function.)

```js
function add(c, d) {
  return this.a + this.b + c + d;
}

var o = {a: 1, b: 3};

// The first parameter is the object to use as
// 'this', subsequent parameters are passed as 
// arguments in the function call
add.call(o, 5, 7); // 16

// The first parameter is the object to use as
// 'this', the second is an array whose
// members are used as the arguments in the function call
add.apply(o, [10, 20]); // 34
```

### `new` operator and Constructors

- JavaScript objects are bags of named properties you can read and set

- Functions are also objects(they can have properties and methods like any other objects). What distinguishes them from other objects is that they can be called. <font color="red" size=4><mark>NOTE</mark>: They are Function objects.</font> Every function in JavaScript is a `Function` object.
1. **Creates a new object**. The type of the object is simply *object*

2. It sets this new object's internal, inaccessible, *[[prototype]]* (i.e. **__proto__**) property to be the constructor function's external, accessible, *prototype* object (every function object automatically has a *prototype* property).
   
   > No clue what the hell this means

3. Makes `this` variable point to newly created object

4. Executes the constructor function(this is the function that new is called on), using the newly created object whenever `this` is mentioned. This is why it first creates the object, binds it, and now the constructor `this` calls refer to the new objects

5. Returns the newly created object, unless the function returns a non-null object reference.

```javascript
function Hero(heroName, realName) {
  this.realName = realName;
  this.heroName = heroName;
}
const superman= Hero("Superman", "Clark Kent");
console.log(superman);
```

*If we run this code not in strict mode, then it will consider this as global. Otherwise it will consider this as undefined. This is a good thing because it prevents us from creating global variables*

**This works**

```javascript
const superman = new Hero("Superman", "Clark Kent");
console.log(superman);
```

* When a function is used as a constructor (with the [`new`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new) keyword), its `this` is bound to the new object being constructed.

* When we call new, a new object is created and set as the function's this argument. The object is then implicitly returned from the function, unless we explicitly return an object.

* The only scenario where the return statement doesn’t overwrite the 
  constructor call is if the return statement tries to return anything 
  that is not an object. In this case, the object will contain the 
  original values.



## Classes



Instead of making 'instances' or copies of classes, JS works ona **delegation** framework. If a property cannot be found in an object, JS looks for that property in a *delegate object*. Delegate objects can be chained(think of this the same way as the inheritance chain.)

* <mark>Prototypes are objects</mark> that are used by other objects to **add delegate properties**

* They are **not** superclasses- no instances are created.
  
  * **An object will just have a *reference* to its prototype**(i.e no instances are created, you just point to an existing prototype)
  
  * **Multple objects can have the same prototype object reference**(so if you modify a prototype, that will change delegate properties for ALL of the objects which have that prototype.)
  
  * An object can have multiple prototypes

If you want to access an object property/methos, JS first looks at the object, if it can't find it in the object, looks at the prototype of the object, if can't find it in the prototype of the objec,t looks in the prototype of the prototype of the object, all the way till it gets to the Object Prototype. 
