## Getting Started with Index Signatures

In this blog post we will be learning about a concept in typescript - Index signatures. 

## What is **Index Signatures :**

Index signature is used to represent the type of object/dictionary when the values of the object are of consistent types.

**Syntax**:  `{ [key: KeyType] : ValueType }`

Assume that we have a theme object which allows us to configure the color properties that can be used across the application. The values will be consistent of type **string.** This makes a perfect opportunity  ****for us to **apply index signature.** 

```jsx
let colorsTheme = {
  palette: {
    success: {
      main: "green",
    },
    error: {
      main: "red",
    },
    warning: {
      main: "orange",
    },
  },
}
```

let’s see how we can add type definition for the below **dictionary** 

```jsx
let ColorsTheme : {
  [key: string]: {
    [key: string]:{
      [key: string]: string
    }
  }
}

 ColorsTheme = {
  palette: {
    success: {
      main: "green",
    },
    error: {
      main: "r...

```

[Playground Link](https://www.typescriptlang.org/play?#code/DYUwLgBAwg9sMCcDOAVAFiAtiCAuCA3gFAQQDaA1iAJ75JgICWAdgOYC6+xpplNdDFh1zcevKrQj0mbTlMFsSPAL5LVqkrHjJ0WHAF5CSgA4BDUGDAguS0kgCuAY0cgkSG2NKZTLfACJWBBAQZj8AGlsIZQixEAQERA9Pb18IPyCAE3DI6MiAd1MEZiEksRTmf0RTNhBssVzSXNUgA)


**What if we try to add a value of different type other than the type defined in the index signature ?
** <br/>

If we try to add a value of type `number` then typescript starts shouting. Take a look at the example below

![https://media.giphy.com/media/l3vR9RE5XzmCeuk6c/giphy.gif](https://media.giphy.com/media/l3vR9RE5XzmCeuk6c/giphy.gif)

```jsx
let ColorsTheme : {
  [key: string]: {
    [key: string]:{
      [key: string]: string
    }
  }
}

 ColorsTheme = {
  palette: {
    success: {
      main: "green",
    },
    error: {
      main: "red",
    },
    warning: {
      main: 1231313,
    },
  },
}
```

In the above example , we have defined the type of the value in the index signature to be string but for the key `main` we gave the value as `1231313` which is a number. 

The error which we will be getting from typescript,

```jsx
(property) main: number
Type 'number' is not assignable to type 'string'.(2322)
input.tsx(61, 7): The expected type comes from this index signature.
```


How do we solve the above error.  We modify the type definition of `ColorsTheme` to accept a string or number . 

```jsx
let ColorsTheme : {
  [key: string]: {
    [key: string]:{
      [key: string]: string | number 
    }
  }
}

 ColorsTheme = {
  palette: {
    success: {
      main: "green",
    },
    error: {
      main: "red",
    },
    warning: {
      main: 1231313,
    },
  },
}
```

*Note: However, properties of different types are acceptable if the index signature is a union of the property types —* [Typescript docs](https://www.typescriptlang.org/docs/handbook/2/objects.html#index-signatures)

```jsx
interface NumberOrStringDictionary {
	[index: string]: number | string | boolean;
	length: number; 
	name: string; 
	isVisible: boolean; 
	
}
```

**What if we try to add the key other than a string or number or symbol for index signature ?
** <br/>

If we try to add type as boolean for key in the index signature , typescript starts shouting again because An index signature parameter type must be 'string', 'number', 'symbol', or a template literal type

```jsx
let foo: {
  [key: boolean] : number // Error : An index signature parameter type must be 'string', 'number', 'symbol', or a template literal type
} 

foo = {
  bar: 1
}
```

## Quick Recap:

Index signature can be used define the type of the object whose values are of consistent types or you don’t know the structure of the object you are dealing with. 

## Conclusion:

That's pretty much it. Thank you for taking the time to read the blog post. I hope , everyone understood about index signature concept. 

If you found the post useful , add ❤️ to it and let me know if I have missed something in the comments section. Feedback on the blog are most welcome.

Let's connect on twitter : (**[https://twitter.com/karthik_coder](https://twitter.com/karthik_coder)**)

## References:

1. [Typescript Docs](https://www.typescriptlang.org/docs/handbook/2/objects.html#index-signatures)
2. [Index Signatures in Typescript](https://dmitripavlutin.com/typescript-index-signatures/)