## Javascript Shorts : null and undefined

Hello everyone, In this blog we will be seeing about the javascript’s two famous primitives **null** and **undefined**. 

## What is null ?

> primitive value that represents the intentional absence of any object value.  - [ECMA - 262](https://262.ecma-international.org/12.0/#sec-null-value)
> 

- javascript never assigns a null value unless and until it is intentionally assigned.
- null is considered as a **falsy** value as it represents there is no value present.

```javascript

let a = null;

console.log(a) // null
console.log(typeof null) // object
console.log(!!a) // false
```

## What is undefined ?

> primitive when a variable has not been assigned any value. - [ECMA - 262](https://262.ecma-international.org/12.0/#sec-undefined-value)
> 

**undefined** is where we declare a variable but not assign any value to it. 

```javascript
let a;

console.log(a) // undefined
console.log(typeof a) // "undefined"
```

## Why typeof null is an object ?

let’s try to understand why when we log the typeof **undefined** we get `undefined` and when we log the typeof null we get an `object` . Since `null` is a primitive value with a primitive type `Null` why not the type as `Null` . 

![https://media.giphy.com/media/gd09Y2Ptu7gsiPVUrv/giphy.gif](https://media.giphy.com/media/gd09Y2Ptu7gsiPVUrv/giphy.gif)

**Typeof** operator returns the type of variable in the form of string. 

```javascript
typeof null === 'object';
```

In the earlier implementation of javascript, values are stored in 32bit units. first three bits ( 1 - 3 ) bits for identifying the type of the value and rest of 29 bits contained the actual value. 

There was 5 type tags in which object had type tag of `000`. In most of the platforms , null was represented as the `NULL` pointer which ( 0x00 ). This loosely translates that `null` also has `0` type tag which is type of `"object”`.  

More info with code on the typeof null : [reference](https://2ality.com/2013/10/typeof-null.html) 

 **MDN Reference**

> In the first implementation of JavaScript, JavaScript values were represented as a type tag and a value. The type tag for objects was `0`. `null` was represented as the NULL pointer (`0x00` in most platforms). Consequently, `null` had `0` as type tag, hence the `typeof` return value `"object"`. ([reference](https://2ality.com/2013/10/typeof-null.html))
> 

> A fix was proposed for ECMAScript (via an opt-in), but [was rejected](https://web.archive.org/web/20160331031419/http://wiki.ecmascript.org:80/doku.php?id=harmony:typeof_null). It would have resulted in `typeof null === 'null'`.
> 

## Interesting facts on null and undefined :

1. they both are equal and not equal. What do I mean by that
    
    
  
```javascript
console.log(null === undefined) // false
```        

When we use javascript’s strict equality operator to check we will get false as the type of both **null and undefined** are different. 

```javascript
console.log(null == undefined) // true
```

Language [spec](https://tc39.es/ecma262/#sec-abstract-equality-comparison) says that when we check for similarities between **null** and **undefined**  using loose equality operator it is should be **true.**

1. what if I try to add **null** and **number. null gets converted to 0 resulting a the number**
    
    
  
```javascript
console.log(1+null) // 1
```     

1. When we try to add **undefined** and number we will get `NaN.`
    
    
```javascript
console.log( 1 + undefined ) // NaN
```

## Conclusion

That's pretty much it. Thank you for taking the time to read the blog post. If you found the post useful , add ❤️ to it and let me know in the comment section if I have missed something. 

Feedback on the blog is most welcome.

Social Links:

[Twitter](https://twitter.com/karthik_coder)
[Showwcase](https://www.showwcase.com/karthik-codes)

## References and Resources:

1. [null vs undefined](https://codeburst.io/javascript-null-vs-undefined-20f955215a2)
2. [loose equality comparison](https://tc39.es/ecma262/#sec-abstract-equality-comparison)
3. [Falsy - MDN docs](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)
