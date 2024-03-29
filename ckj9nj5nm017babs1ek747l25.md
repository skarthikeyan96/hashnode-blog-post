## Javascript Closures in Loops

In this blog, We will be going over the usage of the closures concept in javascript when working with loops.

I have written a blog explaining, how closure works under the hood. Please give it a read before jumping in.  

Getting Started with Javascript Closure -  [Article Link](https://imkarthikeyans.hashnode.dev/getting-started-with-javascript-closure) 

If you are already familiar with the concept of Closure, then let's dive in.

![giphy](https://media.giphy.com/media/Ln2dAW9oycjgmTpjX9/giphy.gif)

A quick definition of Closure

> Closure is when a function remembers the scope where it has been created even when the function is executed in a different scope.

```jsx

const trap = 'It is a trap'

function foo(){
 const inside = 'I have access'
  return function bar(){
    console.log(trap +'but '+ inside)
   }
}

/* `inside` will be garbage collected the moment `foo()` is executed */

const goo = foo() 

/*  Magic happens because of closure as it will have access to the value of the variable inside. */

goo() // It is a trap but I have access

```

There was a misconception when using closure inside the loop.  Let's take a look at the example, 

```jsx
var LIMIT = 3;

function multiplyBy3() {
  for (var i = 1; i <= LIMIT; i++) {
    setTimeout(function () {
      console.log(i * 3);
    }, 1000);
  }
}

multiplyBy3()
```

You would have expected the output to

```
3
6
9
```

But when you ran this on this console the output would have been

```
12
12
12
```



Let's break it down shall we,

`LIMIT` will be created inside the global memory with a value of `3`. When the pointer moves to the next line, the function definition will be referenced under `multiplyBy3`.

When the pointer hits the function call, `multiplyBy3` will be pushed to the `call stack`.  A new execution context will be created for the function with its own local memory. 

Since `setTimeout` is not a part of the Javascript runtime, browser APIs will take care of spinning the timer for `1ms` and push it to the callback queue. The anonymous callback function that is, the closure inside the setTimeout function will be waiting in the `callback queue` for its turn to enter the `call stack` for its execution. 

So similarly, three closures will be waiting in the callback queue. Since they are created inside the function, they will have the same lexical environment shared by all of the closures.

One more crucial note here is since they share the same single lexical environment or in this case single variable `i`  which will be changing for every iteration.

Since the variable is declared with the `var` keyword it will be hoisted to the top and it will be accessible within the function. 

The code will look somewhat like this

```jsx
var LIMIT = 3;

function multiplyBy3() {
	var i;
  for (i = 1; i <= LIMIT; i++) {
    setTimeout(function () {
      console.log(i * 3); // i will be 4
    }, 1000);
  }
console.log(i) // 4
}

multiplyBy3()
```

The value of `i` will be determined when the anonymous function blocks are executed. Since the loop has already executed, the value of `i` will be pointing to `4`. This will result in output `12` for three times.

Let's see the ways we can solve this problem

![https://media.giphy.com/media/fxUJDxHZUQ5P7kRpKI/giphy.gif](https://media.giphy.com/media/fxUJDxHZUQ5P7kRpKI/giphy.gif)

## Solution 1: Using  let Keyword:

The `let` keyword introduced in ES2015 will do the trick. When we replace the `var` keyword with `let`, in the for loop, it will create the block scope, and the new copy of `i` will be created and attached to the anonymous function block every time. 

```jsx
let LIMIT = 3;

function multiplyBy3() {
  for (let i = 1; i <= LIMIT; i++) {
    setTimeout(function () {
      console.log(i * 3);
    }, 1000);
  }
}

multiplyBy3()
```

If we try to access it outside of the for loop, we will end up with the `undefined` error.

```jsx
let LIMIT = 3;

function multiplyBy3() {
  for (let i = 1; i <= LIMIT; i++) {
    setTimeout(function () {
      console.log(i)
      console.log(i * 3);
    }, 1000);
  }
console.log(i) // undefined
}

multiplyBy3()
```

## Solution 2:  Wrapping it up with IIFE

The second solution would be wrapping up the IIFE (immediately invoked function expression ) pattern.  That is, we put all the code inside our loop into a function, and then call that function immediately. 

 

```jsx
let LIMIT = 3;

function multiplyBy3() {
  for (var i = 1; i <= LIMIT; i++) {
    (function(){
			var j = i;
      setTimeout(function () {
         console.log(j * 3);
      }, 1000);
    })();
  }
}

multiplyBy3()
```

## Solution 3: Extract the inner body of the loop to function

The final solution will be to use the function factory. The function `calculate(i)` will create a new lexical environment for each callback in which `i` refers to the corresponding value of the loop.

```jsx
var LIMIT = 3;

function multiplyBy3() {
  for (var i = 1; i <= LIMIT; i++) {
    calculate(i);
  }
}

function calculate(i) {
  setTimeout(function () {
    console.log(i * 3);
  }, 1000);
}

multiplyBy3()
```


![mp4.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1609139377776/FnvnZWx3U.gif)

That's it, folks. Thanks for sticking around. If I have missed something, Let me know in the comment section. I will add it in.  Hit the 👍 or other emojis if you have liked the blog. More on the way.

Stay Safe and Happy Coding 

Resources:

1. [Closures — MDN](https://www.notion.so/Usage-of-Javascript-Closure-inside-For-loop-4a071e0ea667497f8a0785ca1c3086d5)
2.  [JS Visualiser](http://latentflip.com/loupe)
3. [Getting Started with Closure in Javascript](https://imkarthikeyans.hashnode.dev/getting-started-with-javascript-closure)