---
title: 'Execution Context & the Secret Life of Functions'
date: '2019-06-30T22:12:03.284Z'
---
Understanding the Execution Context in one of the most vitally fundamental parts of learning Javascript. This is because understanding the Execution Context paves the way to comprehend more advanced topics like hoisting, scope chains, and closures with clarity. So in this piece, I will be covering the complete life cycle of the Execution Context.

##What is Execution Context?

Execution Context is the way in which the javascript engine modularizes the process of interpreting and running our code; thereby reducing the complexity of the process. There are two types of Execution Context :

- Global Execution Context
- Function Execution Context

##Global Execution Context

Whenever we run a javascript file, even in the case of an empty javascript file by default the engine creates the Global Execution Context for us. Initially, this Execution Context will consist of two things - a global object and a variable called '[this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)', and it will reference the global object which will be 'window' if you're running JavaScript in the browser or 'global' if you're running it in a Node environment.

![](https://thepracticaldev.s3.amazonaws.com/i/n0nf2uoyblcjq8vr49if.png)

When we have variables and functions in our Execution Context, the code undergoes a two-stage process by default and the two phases are:

- The global creation phase.
- The global executing phase.

###Creation Phase will do the following:

1. Create a global object for us.
2. Create an object called "[this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)" and initializes to window/global base on our, environment.
3. Creates the Variable environment that is the memory space for variables and functions.
4. Initializes all 'var' identifier declarations to "undefined" and places all function declarations in memory.

![](https://thepracticaldev.s3.amazonaws.com/i/u459a4xifwb20impw7ep.png)

In the Execution phase, the JavaScript engine starts running our code line by line and executing it.

![](https://thepracticaldev.s3.amazonaws.com/i/c7ybal1b8wp82siagsd1.png)

###Note: 

This process of assigning variable declarations a default value of 'undefined' during the creation phase is called [Hoisting](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting).

```javascript
console.log(firstName); //logs -->undefined 

//it does not throw an error but logs -->undefined;
//this happens because of hoisting happening during the creation phase
var firstName = "Rick";
console.log(firstName); //logs -->Rick
```
##Function Execution Context

Function Execution Context is almost exactly identical to the Global Execution Context with a little difference. Only whenever we invoke any function a function Execution Context is created.

Even the Function Execution Context undergoes a two-stage process by default and the two phases are identical to the Global Execution Context they are:
1. The creation phase.
2. The executing phase.

###Creation Phase will do the following:

Even in Function Execution Context's creation phase is similar to the Global Creation phase but it changes the first step so instead of creating a global object it creates the arguments object with the received arguments. So the steps happening in the creation phase are:

1. Create an argument object with the received arguments.
2. Create an object called '[this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)' and point to the callee or in the absence of callee the to the window/global object.
3. Creates the Variable environment that is the memory space for local variables and functions.
4. Initializes all 'var' identifier declarations to "undefined" and places all function declarations in memory.

![](https://thepracticaldev.s3.amazonaws.com/i/atxcrx19cx4c2ndj8zzc.png)

In the Execution phase again, the JavaScript engine starts running our code line by line and executing it.

![](https://thepracticaldev.s3.amazonaws.com/i/bp93v422pw35e5dfntc6.png)

After when a function execution is over, that is by implicit/explicit return statement the function Execution Context gets deleted with all its variable environment(not always there is a special case called closures that we will see later) and becomes garbage for the garbage collector.

![](https://thepracticaldev.s3.amazonaws.com/i/5nvu7655etonexht22gk.png)

##Call Stack

The JavaScript engine creates an 'Execution Stack' (also known as the "Call Stack") with Global Execution Context as its first or bottom-most item. Anytime a function is invoked, a new Execution Context is created and added to the Execution Stack. Whenever a function is finished running through both the Creation and Execution phase, it gets popped off the Execution Stack.
So a nested Execution Context will look like this:

![](https://thepracticaldev.s3.amazonaws.com/i/mtsdy5lyka61ksrrzeww.png)

The Secret Life of functions does not end here there are more interesting things like [scopes](https://developer.mozilla.org/en-US/docs/Glossary/Scope) and magical-like [closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures). Do check them out.

###Resources:

- [JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/javascript)
- [Javascript The Hard-parts by William Sentance](https://frontendmasters.com/courses/javascript-hard-parts/)
- Images were taken using [JavaScript Visualizer](https://tylermcginnis.com/javascript-visualizer/) by Tyler Mcginnis
