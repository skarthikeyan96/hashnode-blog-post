---
title: "Introducing Odyssey - Personal Journal"
datePublished: Tue Aug 01 2023 07:58:02 GMT+0000 (Coordinated Universal Time)
cuid: clks0bi9x000c09mi82yfes4k
slug: introducing-odyssey-personal-journal
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/cckf4TsHAuw/upload/207ac9c966f384351bb09069fa1b0657.jpeg
tags: awsamplify, awsamplifyhackathon

---

In this post, I will be sharing the tech stack, the process and the challenges I faced during the development.

# Tech Stack :

1. Next.js
    
2. Amplify CLI
    
3. Appsync API
    
4. S3 storage
    
5. Tailwind CSS
    

  
Before getting into the process, the main goal for the hackathon is to build an application that can be used as a personal journal using aws-amplify.

# Process :

The first part was to finish the authentication, luckily Amplify's pre-built ui components came in handy. Below is the code

```javascript
import { Authenticator, useAuthenticator } from '@aws-amplify/ui-react'
import '@aws-amplify/ui-react/styles.css'
import Navbar from '@/components/navbar'
import Wrapper from '@/components/wrapper'
import { Auth } from 'aws-amplify'
import { useRouter } from 'next/router'
import React from 'react'
import { Hub } from 'aws-amplify'

const Profile = () => {
  const router = useRouter()
  const { authStatus } = useAuthenticator((context) => [context.authStatus])
  const { user } = useAuthenticator((context) => [context.user])

  Hub.listen('auth', (data) => {
    switch (data.payload.event) {
      case 'signIn':
        router.push('/')
        break
      case 'signUp':
         router.push('/')
        break
      case 'signOut':
        console.log('user signed out')
        break
      case 'signIn_failure':
        console.log('user sign in failed')
        break
      case 'configured':
        console.log('the Auth module is configured')
    }
  })
  return (
    <>
      <Navbar />
      <Wrapper>
        <div className="p-4">
          {authStatus === 'configuring' && 'Loading...'}

          {authStatus !== 'authenticated' ? (
            <Authenticator />
          ) : (
            <>
              <main>
                <h1> Username: {user?.username}</h1>
                <button
                  className="mt-4 py-3 px-4 inline-flex justify-center items-center gap-2 rounded-md border border-transparent font-semibold bg-gray-800 text-white hover:bg-gray-900 focus:outline-none focus:ring-2 focus:ring-gray-800 focus:ring-offset-2 transition-all text-sm dark:focus:ring-gray-900 dark:focus:ring-offset-gray-800"
                  onClick={() => {
                    Auth.signOut()
                    router.push('/home')
                  }}
                >
                  Sign out
                </button>
              </main>
            </>
          )}
        </div>
      </Wrapper>
    </>
  )
}

export default Profile
```

![https://media.giphy.com/media/8xomIW1DRelmo/giphy.gif](https://media.giphy.com/media/8xomIW1DRelmo/giphy.gif align="left")

One small hurdle I had was to figure out how to redirect after the user has signed up or signed in as I was using the pre-built UI components.

The reason was once the user was logged in, nav components were not getting updated and finally, aws amplify [docs](https://docs.amplify.aws/guides/authentication/listening-for-auth-events/q/platform/js/) came in handy for that.

Next up was to finish up the CRUD operations for the journal. I was able to achieve them with Appsync and Graphql. I also got a chance to refresh my graphql knowledge.

For the creation part of the journal, I made use of the Monaco editor paired up with the tailwind typography plugin to make it even look nicer.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1690875733927/6a1c0325-9c11-4092-8771-c9ba0c204a8f.png align="center")

For adding the cover image for the journal part , I was able to make use of the inbuilt storage provided by AWS S3.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1690875845651/ae66eb73-8e11-4c69-9e9a-65b2008eba22.png align="center")

What's Next for Odyseey

1. Ability to share the journal link with other users.
    
2. Import/export journal content from the Notion.
    
3. Ability to comment on the public journal.
    

## Retrospective:

From this hackathon, I was able to learn how to create a CRUD application with aws-amplify CLI and refreshed my graphql skills.

# Conclusion

Thank you for tuning in. The application will be under continuous development. Comments and Feedbacks are most welcome. More posts regarding the setup which I learned in the hackathon are on the way.

# Links

Web Application: [https://main.d369lw20lvaxqm.amplifyapp.com/](https://main.d369lw20lvaxqm.amplifyapp.com/)  
Source Code: [https://github.com/skarthikeyan96/odyseey-amplify](https://github.com/skarthikeyan96/odyseey-amplify)