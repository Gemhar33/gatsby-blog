---
title: 'var, let and const in JavaScript - Decoded...'
date: '2019-06-22T22:12:03.284Z'
---
While I was getting the hang of the fundamentals of JavaScript, I came across three ways of declaring a variable, that is by _var_, _let_ and _const_ statements. So in this piece, I have tried to summarize all my findings to differentiate each of the declaration statements.

To really get a grip on the differences between _var_, _let_ and _const_ we must apprehend the following four concepts:

- Variable declaration
- Variable initialization
- Scope
- Hoisting

## Variable declaration

Variable declaration is the process of introducing a new identifier into our program; to be specific to our scope(I will talk about scopes later). In JavaScript, identifiers are by default given a value of undefined when declared with _var_ keyword(this is automatically done by the interpreter).

```javascript
var foo; // declaration
console.log(foo); // logs-->undefined
```

## Variable initialization
Variable initialization is the process of assigning values to the identifier initially, so when we declare a binding with _var_ keyword the interpreter automatically initializes it to undefined.
	
```javascript
var foo; //declaration
console.log(foo); // logs -->undefined

foo = "something"; // Initialization
console.log(foo); // logs -->something
```

## Scope 
The scope of a variable actually defines the context in which the variables and functions are accessible and can be referenced in a program. The scope defines the visibility and lifetime of variables and parameters. If a variable is not "in the current scope," then it is unavailable for use. Scopes can also be layered in a hierarchy, so that child scopes have access to parent scopes, but not vice versa.
	
Basically, there are two types of scoping
- function scope
- block scope

### function scope:
Variables declared inside a function are scoped to the function and all the subsequent nested function; irrespective of blocks;
			
```javascript		
function foo() {

  if(true) {
    var v = "var inside the if block";
    console.log(v);  //logs -->var inside the if block
  } 
  v = "var outside the if block";
  console.log(v);  //logs -->var outside the if block
}

foo();
```
			
### block scope
Variables declared inside a block are scoped only to its block and all the subsequent nested blocks but not outside of the block not even in the same function; blocks here include if...else blocks or looping blocks.
						
		
```javascript			
function bar() {

  if(true) {
    let l = "let inside the if block";
    console.log(l);  //logs -->let inside the if block
  }

console.log(l); // Uncaught Reference Error: l is not defined
}

bar();
```

## Hoisting:

MDN defines Hoisting as :
 
>"Hoisting is a term you will not find used in any normative specification prose prior to ES2015 Language Specification. Hoisting was thought up as a general way of thinking about how execution contexts (specifically the creation and execution phases) work in JavaScript. However, the concept can be a little confusing at first.

>Conceptually, for example, a strict definition of hoisting suggests that variable and function declarations are physically moved to the top of your code, but this is not in fact what happens. Instead, the variable and function declarations are put into memory during the compile phase, but stay exactly where you typed them in your code."

```javascript
console.log(foo); //logs -->undefined 

//it does not throw an error but logs -->undefined;
//this happens because of hoisting
	
var foo = "something"; //Initialization
console.log(foo); //logs -->something
```
For the above code, how JS interpreter evaluation can be simplified as :


```javascript
var foo; // Hoisted declaration of 'foo'

console.log(foo); logs -->undefined;
foo = "something";
console.log(foo); //logs -->something
```

## **var**
The *_var_* statement declares a variable, optionally initializing it to a value. Any variable declared with _var_ statement is function scoped and also identifies declared with _var_ keywords are hoisted and initialized with undefined

```javascript
console.log(foo); //logs -->undefined
var foo;
		
//the above code does not throw an error because of hoisting;
```

## **let** 
The *_let_* statement declares a local variable. Any variable declared with _let_ statement is block scoped. The identifies declared with _let_ keywords are hoisted and are not initialized

```javascript
let foo;
console.log(foo); // Uncaught Reference Error: l is not defined
			
//the above code throws an error because identifiers declared with let keywords are not initialized;
```
			
_let_ bindings are created at the top of the (block) scope containing the declaration, commonly referred to as "hoisting". Unlike variables declared with var, which will start with the value undefined, let variables are not initialized until their definition is evaluated. Accessing the variable before the initialization results in a Reference Error.

## **const**
The _const_ statement declares a local variable very much like the _let_ statement but it has one added property and that is; they cannot be reassigned; meaning once the _const_ binding is initialized it cannot be reassigned with any other value. 	
Due to the above reason, a _const_ binding will always have to be initialized when declared otherwise it throws an error.

```javascript 
const foo = "something";
foo = "other thing"; // Uncaught TypeError: Assignment to constant variable.	
		
const bar; //Uncaught SyntaxError: Missing initializer in const declaration
		
```

**NOTE:**
One thing to observe here is that when we use _const_ binding to an object, the object itself cannot be changed and will continue to point at the same object, the contents of that object might change.

```javascript
const score = {visitors: 0, home: 0};
    
score.visitors = 1; // This is okay  
score = {visitors: 1, home: 1}; // Uncaught TypeError: Assignment to constant variable.
// This isn't allowed
```

## One last fun fact:

Bindings that are declared in a function without a declaring keyword will become a global variable. Let me explain this with an example:

```javascript
function funFact() {
  isGloballyAvailable = true;
}

funFact();
console.log(isGloballyAvailable); // logs true
```
To understand this we must go back to our hoisting concept, usually what happens is that whenever we initialize a variable in our code the interpreter goes and searches the hoisted variables and then assigns or reassigns the value of the variable, but when the interpreter cannot find the variable in the function it goes and searches in its parent function's hoisted variables and this process repeats until the global scope; 

 In our case the interpreter will not find our 'isGloballyAvailable' binding even in the global scope so, the interpreter automatically adds the variable to the global scope.

 This is an extremely dangerous process and must be avoided at all cost; so keep in mind that we must not declare a binding without the: _var_, _let_ or _const_ keyword anywhere in our code.

## **So when should we use _var_, _let_ or _const_ ?**


ES2015 (ES6) introduced _let_ and _const_, why would the JavaScript designers introduce them? maybe to fix some issue with _var_ or maybe for better readability... right? 

One major issue with _var_ is that it allows for re-declarations in code, which does not throw errors, which can create unintended side-effects in your code. 

### The popular and as well as my opinion is that :
We should always prefer _const_ if the value assigned to our variable is not going to change, this tells the future developers that the identifier has a constant value.
On the other hand use _let_ if the identifier needs to change its value later on, but I do not see a use case in which we should use _var_.	