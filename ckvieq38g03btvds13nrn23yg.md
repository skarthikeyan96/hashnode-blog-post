## How to add dark mode toggle to Next.js application using Chakra UI

In this short blog post , I will be walking us through how we can add dark mode toggle to Next.js application using chakra ui. 

Chakra ui is a component library for building front end part of your application.We can customise and reuse the component as per our needs. 

Let's see how we can add dark mode to our application using chakra ui. 

![https://media.giphy.com/media/2GjgvS5vA6y08/giphy.gif](https://media.giphy.com/media/2GjgvS5vA6y08/giphy.gif)

## Bootstrapping our Next.js app:

We can use `create-next-app` with typescript to bootstrap our project. Head over to your terminal and type in the following command

```bash
create-next-app next-chakra-dark --ts

cd next-chakra-dark

yarn dev
```

The above command will create a brand new next application with inbuilt and customisable `tsconfig.json`.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1635875397470/LIkcSpQE7.png)


And we got our boilerplate application ready for us to add dark mode toggle. Head over to the [localhost:3000](http://localhost:3000) to see the action.

Let's setup chakra ui. 

## Setting up Chakra UI:

Now head over to terminal and type in the following command to setup chakra ui 

```bash
yarn add @chakra-ui/react @emotion/react@^11 @emotion/styled@^11 framer-motion@^4
```

For Chakra UI to work correctly, you need to set up the `ChakraProvider` at the root of your application.

Head over to `pages/_app.tsx` and type in the following. If the file does not exist , please create one. 

```jsx
import '../styles/globals.css'
import type { AppProps } from 'next/app'
import { ChakraProvider } from "@chakra-ui/react"

function MyApp({ Component, pageProps }: AppProps) {
  return (
    <ChakraProvider>
      <Component {...pageProps} />
    </ChakraProvider>
  )
}

export default MyApp
```

Since my browser has dark mode by default , chakra is using the value to set the mode. We will change that by extending the theme file.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1635875427417/eoanhr9K0.png)


## Extending Chakra's theme:

Create a `theme.ts` in the root of your project and paste in the following content

```jsx
// theme.ts

// 1. import `extendTheme` function
import { extendTheme, ThemeConfig } from "@chakra-ui/react"

// 2. Add your color mode config
const config : ThemeConfig = {
  initialColorMode: "light",
  useSystemColorMode: false,
}

// 3. extend the theme
const theme = extendTheme({ config })

export default theme
```

In the above code , we have created our own config which sets the initial color mode to `light` and makes sure chakra does not uses systems color which was set to dark in my case. 

Since the app is already running on `[localhost:3000](http://localhost:3000)` head over and then check to see if anything has changed.  It will look exactly the same. 

We have pass our new extended theme as prop to our `chakraProvider` which we have setup in the `_app.tsx` file. Head over to file and make one small change.

```jsx
import '../styles/globals.css'
import type { AppProps } from 'next/app'
import { ChakraProvider } from "@chakra-ui/react"
import theme from '../theme' 

function MyApp({ Component, pageProps }: AppProps) {
  return (
    <ChakraProvider theme={theme}>
      <Component {...pageProps} />
    </ChakraProvider>
  )
}

export default MyApp
```

Now head over to the browser to see the change.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1635875448403/lt6REbqgL.png)



Now let's make the toggle switch. Chakra UI has a component called `colorModeScript` which will keep track of the color modes light, dark or system default. `ColorModeScript` will take in prop `initialColorMode` which can have either of above mentioned three values. Light mode would be default one. 

Since `colorModeScript` adds in a script element. , it is recommended as per the docs it has to be in our root of the application. In Next.js there is a way we can create our own custom document. 

Go to `pages` folder and create `_document.tsx` file and add in the following content

```jsx
// pages/_document.tsx

import { ColorModeScript } from "@chakra-ui/react"
import NextDocument, { Html, Head, Main, NextScript } from "next/document"
import theme from "./theme"

export default class Document extends NextDocument {
  render() {
    return (
      <Html lang="en">
        <Head />
        <body>
          {/* 👇 Here's the script */}
          <ColorModeScript initialColorMode={theme.config.initialColorMode} />
          <Main />
          <NextScript />
        </body>
      </Html>
    )
  }
}
```

# Systematically Managing colors:

Chakra exposes hook `useColorMode` function which will allow us to change the color mode as preferred. 

**Note:** 

Please run `yarn add @chakra-ui/icons` for using `MoonIcon and Sunicon` used in the code below 

Go to `pages/index.tsx` and add in the following content

```jsx
import type { NextPage } from 'next'
import Head from 'next/head'
import styles from '../styles/Home.module.css'
import { useColorMode } from '@chakra-ui/color-mode'
import React from 'react'
import {  Heading } from '@chakra-ui/layout'
import {
  MoonIcon,
  SunIcon
} from '@chakra-ui/icons';
import { IconButton } from '@chakra-ui/button'

const Home: NextPage = () => {

  // hook which help us to toggle the color modes
  const { colorMode, toggleColorMode } = useColorMode()
  
  return (
    <div className={styles.container}>
      <Head>
        <title>Dark mode using Next.js and Chakra UI</title>
        <meta name="description" content="Dark mode using Next.js and Chakra UI" />
        <link rel="icon" href="/favicon.ico" />
      </Head>

  
      <main className={styles.main}>
       <Heading> Dark Mode toggle using Chakra UI and Next.js </Heading>
       
       <IconButton mt={4} aria-label="Toggle Mode" onClick={toggleColorMode}>
          { colorMode === 'light' ? <MoonIcon/> : <SunIcon/> }
        </IconButton>
       
         
      </main>

      <footer className={styles.footer}>
        Built in ♥️ with Next.js and Chakra UI
      </footer>
    </div>
  )
}

export default Home
```

Head over to the browser to see the final action.

Final Screen ( Dark mode ) 


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1635875496659/afdmVt-Ri.png)


Final Screen ( Light mode )


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1635875520845/9YtvggPH9.png)

Congratulations  👏 👏 👏  . You have added dark mode toggle to your Next.js application using Chakra ui. 


## Conclusion.

Thank you for taking the time to read the blog post. If you found the post useful , add ❤️ to it and let me know if I have missed something in the comments section. Feedback on the blog are most welcome.

Let's connect on twitter : ([https://twitter.com/karthik_coder](https://twitter.com/karthik_coder))

Repo Link : https://github.com/skarthikeyan96/next-chakra-dark 

![https://media.giphy.com/media/A5OPIlNp8fHQbATsvC/giphy.gif](https://media.giphy.com/media/A5OPIlNp8fHQbATsvC/giphy.gif)

# Reference:

1. [Chakra UI docs](https://chakra-ui.com/docs/features/color-mode)