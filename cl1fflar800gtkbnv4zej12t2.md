## Kross post - Final Showdown

In this post ,  I will be sharing the tech stack , the process and challenges I faced during the development

*Note: Here is the link to my introduction [post](https://imkarthikeyans.hashnode.dev/introducing-kross-post-cross-posting-made-easy) about the application
*

# Tech Stack :

1. Next.js
2. Hasura
3. Vercel
4. React Dropzone
5. Tailwind css
6. Eslint and Prettier
7. Auth0
8. Cloudinary

# Process :

First Step was to choose the authentication provider. There was these following auth providers which played well i.,e straightforward and easy configuration with Hasura 

1. Auth0
2. Nextauth 
3. Magic
4. Firebase

I picked Auth0 because there free plan was providing 7000 users and configuration was easier. I made use of the `rules` section of `Auth0` to insert the data to a user table whenever the user register for application. 

Barrier one was complete. 

Next step was integrating `Apollo Client` with Next.js which was actually little bit pain. This closed [issue](https://github.com/auth0/nextjs-auth0/issues/277) from Next-auth0 sdk was helpful in setting up the HOC. In order to make the request from the client secure I had to proxy all my api request to `api/graphql` endpoint

That was another level down. 

![https://media.giphy.com/media/8xomIW1DRelmo/giphy.gif](https://media.giphy.com/media/8xomIW1DRelmo/giphy.gif)

Next step was setting up the Settings form , to get the API key's of the user.  

For this step I made use of the [Tailwind forms](https://tailwindcss-forms.vercel.app/) plugin and `@apollo/client` hooks `useQuery` and `useMutation` to create and fetch the queries from hasura's postgres database. 


Now to the big step, Creation and preview of post 


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648756502200/sSKEm-yFv.png)


Here I made use of `Monaco Editor` and `React Markdown` to allow the users to create markdown content and preview them. 


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648756572493/KEJyqe-oP.png)

For adding cover images ,  I made use of `React dropzone` and **cloudinary** to drag and drop and upload to cloudinary to get the image url. 


Finally when the user publishes the post , they will get redirected to the `Dashboard` where they can view the past posts with dates. 

*Note: The post will be first published to `dev.to` and then the published url will be used as a canonical url to repost the articles in `hashnode` and `medium`. In the next phase of the application , there will be option for users to use choose which blog can be their primary i,.e article will be posted to the blog first and then published url will be taken and used to repost across other platforms. *


<div style="width:480px"><iframe allow="fullscreen" frameBorder="0" height="200" src="https://giphy.com/embed/drPEp5mynCBwPQLgSf/video" width="480"></iframe></div>


Thank you Hashnode and Hasura for the hackathon , learned a lot in the process. 

# Screenshot:

![image](https://user-images.githubusercontent.com/23126394/161131122-47619382-c1cf-4b14-8f74-80c77c63822a.png)

# Demo:

<iframe title="vimeo-player" src="https://player.vimeo.com/video/694580894?h=95eac4c84a" width="640" height="360" frameborder="0" allowfullscreen></iframe>

# Conclusion

Thank you for tuning in. Application will be under continuous development. Comments and Feedbacks are most welcome. More posts regarding the setup which I learned in the hackthon are on the way. 



# Links

Web Application : [Kross-post](https://kross-post-typescript.vercel.app/) <br/>
Source Code : [https://github.com/skarthikeyan96/kross-post-typescript](https://github.com/skarthikeyan96/kross-post-typescript)