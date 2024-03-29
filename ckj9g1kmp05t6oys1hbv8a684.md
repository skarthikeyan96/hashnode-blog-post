## Getting Started With Javascript Closure

In this blog,  I will be explaining one of the important features of JS which is `Closure`, and why we need them. So let's dive in. 

## Closure?

Understanding Closure will help in understanding the other concepts in JS, like Higher-order functions and currying.

Generally, Higher-order functions do either of these two

1. Allows the function to take the functions as an argument 
2. Allows the function to return the other functions.

The feature which we are about to see relates to returning the functions from the other function. What if, in addition to returning the function, we get information along with the function that is being returned.

Let's take a look at an example, 

![image](https://user-images.githubusercontent.com/23126394/99179479-35df0600-2744-11eb-816f-c478399a246b.png)


```javascript

  Outer Scope and Inner Scope

```

You would have thought, like how the `bar` was able to access `outerScope`. It should not be possible, as the instance of the `outerScope` created in the local memory will be erased once the execution of `foo` is complete. There is also no reference to the variable present in the global scope.

But Javascript makes it possible. When the function `foo` is called, both variable `OuterScope` and the function `bar` will be created inside the local memory that shares the same lexical environment. Due to which when `bar` is returned from the `foo` it will have access to the surrounding variables during the time of its declaration. 

![tenor](https://user-images.githubusercontent.com/23126394/99179766-a4bd5e80-2746-11eb-8870-b1366ed3ace3.gif)

A `closure` is the combination of the function and the lexical environment within which it has been created. 

It is an inner function that will remember the scope with which it has been created even if it is executed in a different scope.

### Technical Definition according to MDN 

>Technically, A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the lexical environment).
>In other words, closure gives you access to the scope of outer function from the inner function

How can we view the data returned with the inner function?

Generally, when a function is created, it will have a hidden value `[[scope]]` where it will contain all the information about the data that is being returned with the function.`[[scope]]` is not accessible.  

If we run the following in the chrome dev tools 

```javascript
console.dir(func)
```
We will get the following result in the console.
 
![image](https://user-images.githubusercontent.com/23126394/99179948-1ba72700-2748-11eb-9afe-fe06d77ac077.png)

Now a real-world example on closure, 

```javascript

  function trapA(a) {
    return function (b){
      return function (c) {
         return a * b + c
      }
    }
  }

  console.log(trapA(2)(3)(4)) // 10

```
Same code with slight modification

```javascript

  function trapA(a) {
    return function (b){
      return function (c) {
         return a * b + c
      }
    }
  }

  const wrapper = trapA(2);
  console.dir(wrapper)

  const trapB = wrapper(3);
  console.dir(trapB)

  const trapC = trapB(4);

  console.log(trapC) // 10 

```
Let us break it down. 

1. Once the execution of `trapA` is complete, it returns the function definition of the anonymous function and the value of `a`. It is stored in `wrapper`.

2. `console.dir` of `wrapper` will give the closure details.

3. Upon the execution of the first anonymous function stored in `wrapper`,  the value of `a`, `b`, and `anonymous function` are returned and stored in `trapB`.

4. `console.dir` of `trapB` will give the closure details.

5. Finally, the second anonymous function is executed and the expression is evaluated successfully, as it will have access to `a`,`b`, and `c`.

6. When the final `console.log` statement is executed, the value `10` is returned to the screen.

 Below is the screenshot for the above code snippet that depicts the value stored in `[[scope]]` for every function call.

![image](https://user-images.githubusercontent.com/23126394/99264696-d0277280-2846-11eb-944e-73b1d3748597.png)

## Why closures

With Closures, we can emulate the concept of the private method in Javascript, as they are not available natively. Let's see an example of how we can achieve that via closure

![image](https://dev-to-uploads.s3.amazonaws.com/i/2ffrd3smpus64kuincid.png)

Based on the above code snippet, three functions `fullName, addNum, and getNum` share the same lexical environment, and thanks to Javascript's closure concept it will access the variable `num` and it won't be accessible outside of the function. 


![mp4.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1609139377776/FnvnZWx3U.gif)

That's a wrap on Closure, folks. Thank you for sticking around. Reviews and critics of the blog are always welcome.

 If I have missed something, Let me know in the comment section. I will add it in.  Hit the 👍  or other emojis if you have liked the blog. More on the way.  

Stay Safe and Happy Coding !!!

## Useful Resources

1. [MDN docs on Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
2. [JS visualising tool](https://ui.dev/javascript-visualizer/)

