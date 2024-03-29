## How to change the default port number in Next.js application

Hello everyone,

 In this short blog post , I will be writing about how we can override the default port number in Next.js application. 

## Why am I writing this ?

Today when I was working I had to run two different Next.js application  say `app A` and `app B` . Since the default port of Next.js application was `3000` and I was not able to run `app B` . 

Immediately went online searching for how to change port , it was simple one. 

let's dive in 

![https://media.giphy.com/media/2GjgvS5vA6y08/giphy.gif](https://media.giphy.com/media/2GjgvS5vA6y08/giphy.gif)


## How do we change it ?

let's assume that you have `Next.js` app bootstrapped using manual configuration or using `create-next-app` . 

Go to package.json file of your Next.js app and do the following 

**Note**: Following is the `package.json` from my side project which I have been working on. 

```json
{
  "name": "front-end",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev", 
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "@chakra-ui/icons": "1.1.0",
    "@chakra-ui/react": "1.7.0",
    "@emotion/react": "11.5.0",
    "@emotion/styled": "11.3.0",
    "axios": "0.24.0",
    "framer-motion": "5.0.0",
    "next": "12.0.3",
    "nookies": "2.5.2",
    "react": "17.0.2",
    "react-dom": "17.0.2",
    "react-icons": "^4.3.1"
  },
  "devDependencies": {
    "@types/react": "17.0.34",
    "eslint": "8.0.1",
    "eslint-config-next": "12.0.3",
    "typescript": "4.4.4"
  }
}
```

to 

```json
{
  "name": "front-end",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev -p 3001",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "@chakra-ui/icons": "1.1.0",
    "@chakra-ui/react": "1.7.0",
    "@emotion/react": "11.5.0",
    "@emotion/styled": "11.3.0",
    "axios": "0.24.0",
    "framer-motion": "5.0.0",
    "next": "12.0.3",
    "nookies": "2.5.2",
    "react": "17.0.2",
    "react-dom": "17.0.2",
    "react-icons": "^4.3.1"
  },
  "devDependencies": {
    "@types/react": "17.0.34",
    "eslint": "8.0.1",
    "eslint-config-next": "12.0.3",
    "typescript": "4.4.4"
  }
}
```

Based on the above code, only change which has to be made is to add `-p` in `next dev` with port number at the end.

```
"dev": "next dev -p 3001"
```

![https://media.giphy.com/media/cgITA6yha7hVC/giphy.gif](https://media.giphy.com/media/cgITA6yha7hVC/giphy.gif)

Now go to your terminal and run `yarn run dev` your second Next.js application should be running on port 3001. Go to [http://localhost:3001](http://localhost:3001) url to see the app live on your local machine. 

## **Conclusion:**

That's pretty much it. Thank you for taking the time to read the blog post. If you found the post useful , add ❤️ to it and let me know if I have missed something in the comments section. Feedback on the blog are most welcome.

Let's connect on twitter : (**[https://twitter.com/karthik_coder](https://twitter.com/karthik_coder)**)

![https://media.giphy.com/media/eujb1tWaj3ZxS/giphy.gif](https://media.giphy.com/media/eujb1tWaj3ZxS/giphy.gif)