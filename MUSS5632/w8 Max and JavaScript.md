Module: MUSS5632M Electronic & Computer Music Practice, Week 8
# Max + JavaScript
Tuesday 15 November, 2016 || 2hrs

### Summary:
Working with JavaScript in Max... 💻 🎛 🎧

***
## Table of Contents
- [Background](#Background)
- [Basics](#Basics)
	- [Inlets & Outlets](#inlets-and-outlets)
	- [Hello, World!](#hello-world)
	- [Define Your Own Function](#define-your-own-function)
- [Conditions, Loops and Recursion in JavaScript](#conditions-loops-and-recursion-in-javascript)
	- [The IF/ELSE Condition](#the-if-else-condition)
	- [The WHILE loop](#the-while-loop)
	- [The FOR loop](#the-for-loop)
- [Running a FOR LOOP in Max](#running-a-for-loop-in-max)
- [Losing friends with JavaScript and Collatz Conjecture](#Losing-friends-with-JavaScript-and-Collatz-Conjecture)
- [Advanced JavaScript in Max](#advanced-javascript-in-max)
- [For Next Session](#for-next-session)

***
## Background
Today we're looking at a programming language called **JavaScript**. You've probably heard of it, even if you've never used it.
While it's typically associated with web design, Max allows us to edit and run JavaScript code inside our patches. With JavaScript, *we can approach some things slightly differently than we normally would in Max*.

Don't worry if you've never done any programming before, we'll look at some of the fundamentals as we go along...
If you *have* done some programming in JavaScript (or any other text-based language) try to think about how you could develop some of these skills within the context of Max to produce some more advanced techniques!

Even if you don't use JS in your Max projects, this will serve as a good introduction to thinking about text-based programming when working with music, and how it can alter your perspective on making electronic and computer music.
***
## Basics
JavaScript is a lightweight, interpreted object-oriented programming language that was originally developed to facilitate embedded software in websites. By incorporating JavaScript into our Max patches, we can do a lot of powerful and exciting new things:

+ design procedural operations that would be difficult or even impossible in normal Max patches. (for instance, operations that require recursion, or respond to messages with an unknown number of arguments)
+ manipulate the user interface of Max and draw things!
+ create objects that respond to customised messages
+ schedule timed events in response to messages
+ manage global variables for use between multiple `[js]` objects
+ interface with Max's own scripting architecture
+ access the file system of your computer to look for files

One particularly useful thing is using JS to create and/or parse large data sets so they are easier for Max to read. We'll look at a simple example of this towards the end of the session.

In Max, we have two different "flavours" of JavaScript that can run *within* Max: the `[js]` object and the `[jsui]` object. Both objects run JavaScript (JS) version 1.8.5.
**NOTE:** The JavaScript language used by the `[js]` and `[jsui]` Max objects does not include web browser-specific functions and properties

##### The `[js]` object
`[js]` allows us to execute JS script **inside** our Max patches by exposing JS language and some 'Max specific extensions'.

>The `[js]` object can be instantiated with a javascript filename or with numerical arguments to specify the number of outlets and inlets respectively. ([js](https://docs.cycling74.com/max7/maxobject/js), Max Reference)

##### The `[jsui]` object
`[jsui]` allows us to control user interface elements using JavaScript. It provides the tools of `[js]` but also exposes `mgraphics` and drawing routines for visual output.

For the time being, we're just going to focus on the `[js] `object.

>You might find some useful JS information in the [Mozilla JavaScript reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference).

#### How it works
JavaScript (JS) is an interpreted text-based language. For now, the code within our scripts can either be thought of as either '**Global** code', or a '**function**'.
+ **Global** code is executed immediately when a JS file is compiled and can be 'seen' from all levels of the script. Think of it as the startup code, (or things triggered by a `[loadbang]` object in Max).
+ **Functions** on the other hand *can* be called by the global code, but they can also be called by messages sent to the `[js]` object from outside (Max).

This is an imaginary JavaScript file that is sitting inside an imaginary Max patch...
```javascript
var a = 2;
function assign(x){
	a = x;
}
```
The **global** code tells us that our variable `a` equals 2.
However, the **function** named `assign` can change the value of `a` to whatever argument (x) is sent to it...
Implementing functions in JS is pretty easy, because the name you give a function is the same as the message name you might use it invoke it.

![ex.1](figs/ex1.png)

For example, if JS code above was inside this `[js]` object, clicking the Max message `assign 10` would call the `assign()` function in our JS script with an argument of 10!

***
### Housekeeping: good code practices
Before we go any further, it might be helpful to clear up some housekeeping matters. I'm sure you already know this from your work in Max, but making sure your work is **tidy** and clearly **commented** is invaluable when it comes to revisiting a patch you worked on last week or even last year.

In the same way that it's a nightmare to try an untangle a messy Max patch ("where does this patch cable go again?", "what is this 'bang' doing?!"), messy code can make everything a lot harder to understand and debug when you need to look for problems...

###### Always comment your code
In JavaScript (and a lot of other programming languages), we can write comments in our code that won't be compiled when the code is run. This let's us make notes to ourselves to explain what is going on at certain points.
You can add a comment by adding a `//` before it, or by wrapping a block of comments in `/*` and `*/` tags, like this:

```javascript
// CODE COMMENTS:
// this is a comment on a single line
var a = 0; // the code before this comment will be compiled as normal, but this won't.
var b = 1;
/* and neither
will
this. */

// INDENTING:
function assign(x){
	a = x; // this line is *within* the assign() function, so we indent it one level
} // this brace closes the assign() function, so now our code is back to the global scope level and not indented!
```
###### Indenting
When we start to nest blocks of code, and work with objects, we'll start to indent levels of our code, usually between `{ }` braces. It make things a lot easier to follow the scope of our code if we keep our indents neat and not jumping around. Some languages like Python won't work if the indents aren't in the correct position. Luckily JavaScript does a pretty good job at indenting for us, but as you edit your code, try to keep it clear and clean.

***

### Let's get started
1. Create a new Max patch window.
1. Add a `[js]` object
	+ If you provide a filename (filename.js) as the '[js]' object's first argument, you can point to an existing JavaScript file. If you leave it blank, you'll create a new file.
1. `cmd+double-click` to open a text editor window of our shiny new JavaScript file!
	+ > "O brave new world! That has such JavaScript in it..."

***

### Inlets and Outlets
So we need to give our code some inlets and outlets. In our script, when we initialise our global code, we can set the number of inlets and outlets of our `[js]` object in Max. Later, we can specify which inlet or outlet we want to receive or send our data...

```javascript
// inlets and outlets
inlets = 1;
outlets = 2;
```

### “Hello, World!”
**The BANG() function**
Max's `[js]` object can read an incoming 'bang' much the same as a message, passing it to its own function, creatively titled: `bang()`
```javascript
inlets = 1;
function bang(){ // when `[js]` object receives a bang...
	post("Hello, World!"); // it prints a message to our Max console...
	post();
	// the blank "post()" adds a new line to the end of our message, otherwise "Hello, World!" would keep posting on the same line.
}
```
In Max's JavaScript, a few functions like `bang()` are reserved for special purposed. Other special functions are:
- `msg_int()` : function responds to an integer number
- `msg_float()` : function responds to a floating-point number
- `loadbang()` : function is invoked when `[js]` is first loaded
- `list()` : handle Max lists

There are others as listed in Max's JavaScript reference documentation. Some other useful functions than we can call...
- `post()` : post a message to the Max console window
- `outlet(0,x)` : output data from an outlet (where 0=outlet index and x=variable)
- `return x` will simply return a value (x) when the function it is inside gets called.

### Define Your Own Function
Other than those special named functions, you can create your own function to do any number of things in JavaScript...

```javascript
inlets = 1;
outlets = 1;

function yourOwnFunction(){ // create your own wonderful function!
	post("My Function does something wonderful!"); // your function can do anything you like...
	post();
}
function bang(){
	yourOwnFunction(); // yourOwnFunction() is only called when you 'bang' the [js] object
}
```
If you haven't already, add the code above to your own JS file in Max, and make sure your file is saved and the file name is defined in the `[js]` object... (`[js javascript-test.js]`).

**Take a minute or two to get it working and printing to the Max console when you send a bang to the '[js]' object.**
***

## Conditions, Loops and Recursion in JavaScript
One of the nice things about using JS inside Max is that it allows us to work with recursion in a few simple but effective ways. There are a number of different options, but let's just look at a few different ways of writing conditional loops and recursion...

##### The IF/ELSE condition
If a **condition** is TRUE, then do *something*. otherwise, do *something else*. We can write this in 'pseudo-code' like this:

```
if (condition) {
    // code block executed if condition is TRUE
} else {
	// code block exectued if condition is FALSE
}
```
We could also add another condition which is executed if condition 1 is false, but conditiion 2 is true... instead of just moving to our 'else' code block.

```javascript
if (x < 10) {
    post("x is less than 10");
} else if(x > 20){
	post("x is greater than 20");
} else {
	post("x is somewhere between 10 and 20");
}
```

##### The WHILE loop
Sometimes, while a condition is TRUE, we'll want to *keep on doing something until that condition ceases to be TRUE*.
```
while(condition is true){
	// execute code block
}
```

In the next example, while `i` is a value less than 10, we'll keep adding 1 to the value of `i`. This gives us 10 iterations of a code block where `i` changes each time...
```javascript
while(i < 10){
	post("The number is "+i);
	i++ // "++" increases the value of an integer by 1
}
```

##### The FOR loop
When you want to just run a block of code a number of times, the FOR loop is probably your best friend...
```
for(statement 1; statement 2; statement 3){
	// code block to be executed
}
```
+ `statement 1` is executed **before** the loop starts
+ `statement 2` defines the **condition** for running the loop
+ `statement 3` is executed each time **after** the loop

So let's look at a for loop in action...
```javascript
for(i=0; i<5; i++){
	post("The number is "+i);
	post();
}
```
+ `statement 1` the variable i is set to 0
+ `statement 2` the loop runs while i is less than 5
+ `statement 3` at the end of each loop iteration, add 1 to i

***
### Running a FOR LOOP in Max
So, in out `[js]` object, let's execute a for loop when we send a bang to the object:
```javascript
inlets = 1;
outlets = 1;

function bang(){
	for (i = 0; i < 10; i++) {
		post("The number is " + i);
		post();
	}
}
```
Now, bang it and watch your Max console window!

Let's try interacting with our loop and JS file a little more and developing this...

##### CHALLENGE:
>CAN YOU FIGURE OUT HOW TO CHANGE THE NUMBER OF LOOP REPETITIONS FROM MAX?
>Tip: you'll want to add an extra inlet and pass an integer number in from Max... remember that JS has a special function for reacting to a received number!

***
***
##### SPOILER!
***
###### Don't scroll down until you've tried the challenge!
***
###### Seriously.
***
###### Stop.
***
###### Okay, if you're sure...
```javascript
inlets = 2;

var loopSize;

function msg_int(arg){
	loopSize = arg;
}
function bang(){
	for (i = 0; i < loopSize; i++) {
		post("The number is " + i);
		post();
	}
}
```
***
### Losing friends with JavaScript and Collatz Conjecture
On [Wikipedia](https://en.wikipedia.org/wiki/Collatz_conjecture) the Collatz Conjecture is summarised as:

>Take any positive integer n. If n is even, divide it by 2 to get n / 2. If n is odd, multiply it by 3 and add 1 to obtain 3n + 1. Repeat the process (which has been called "Half Or Triple Plus One", or HOTPO) indefinitely. The conjecture is that no matter what number you start with, you will always eventually reach 1.

>Paul Erdős said about the Collatz conjecture: "Mathematics may not be ready for such problems."



![collatz](https://imgs.xkcd.com/comics/collatz_conjecture.png)

> xkcd's take on the [Collatz Conjecture](https://imgs.xkcd.com/comics/collatz_conjecture.png)

Let's use our new found skills with JavaScript to take play with this conjecture, often know as **HAILSTONE NUMBERS**

First, we'll write out the conjecture in pseudo-code...

```
n = any positive integer
while (n is above 1){
	if n is even{
		divide n by 2
	} else {
		multiply n by 3 and add 1
	}
}
```

##### Challenge!
Try and create a function to run through the Collatz conjecture where `n = 72`...

>**Tips**:
>+ you'll want to define n as a global value.
>+ you'll want to *call* your function from within the bang() function and post(n) to the Max console...

###### Taking it further
If you wanted to output a big list of numbers to Max, rather than spit out individual numbers, consider exploring **Arrays**.

###### And here's the solution:
```javascript
inlets = 1;
outlets = 2;
var seed;
var collatz = []; // create an empty Array

function hailStones(){
	collatz.push(seed); // add our 'seed' value to the Array
	while(seed > 1){
		if(seed%2 == 0){ 			// if i is even
			seed = seed/2;				// divide by 2
			collatz.push(seed);
		} else { 					// or, if it is odd
			seed = (seed*3)+1;			// multiply by 3 and add 1
			collatz.push(seed);
		}
	}
	return collatz;
}

function msg_int(arg){
	seed = arg;
}

function bang(){
	collatz = [];
	outlet(0,hailStones()); // output the Array of hailstone numbers
	outlet(1,collatz.length); // length of Array
}
```

When it works, try shifting the value of 'n' and see how your data set changes! Can you *sonify* this somehow?
***
## Advanced JavaScript in Max
So, WHY use text-based languages?
- Easy to work with **Recursion** and **Conditional** loops.
	+ This is handy when you want to repeat a number of actions, such as parsing a big block of data.
- Go deeper into control of Max objects & let us do things we otherwise could not.
- We didn't look at it today, but `[jsui]` allows us to do powerful things with Max's user interface in JS.
- Run graphical simulations that rely on complex data (rather than with JITTER)
	- ‘Boids’ example
		+ How could we implement this in music?
- Scheduling events.

![BOIDS](figs/JS-Max-boids.gif)

***
## For next session:
- Explore some of the ‘[JavaScript for Max](https://docs.cycling74.com/max7/vignettes/JavaScriptinmax)’ example patches and be sure to read the reference page guides!
- Take a look at Max’s ‘GEN’ language with the ‘[Gen Overview](https://docs.cycling74.com/max7/vignettes/gen_overview)’ guide in the reference pages.

If you need more help with JavaScript (or many other programing languages), check out [CodeAcademy](https://www.codecademy.com)

Next week we enter the wonderful world of [SuperCollider](http://supercollider.github.io)...
`“Hello, World!”.postln;`
