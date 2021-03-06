## How to build a movie application using Next.js and Appwrite ?

In this tutorial we are going to build a movie application using Next.js , Appwrite, tailwind css, Digital Ocean and deploy it to Vercel. 

Before going into coding. Let’s see a few details on the frameworks which we are going to work with. 

## **Introduction:**

**Next.js:**  It is a React framework for building server side applications with more features like static rendering , Page based routing, smart bundling etc. 

**Appwrite**: It is a self-hosted backend-as-a-service platform that provides developers with all the core APIs required to build any application.

**Tailwindcss**: Tailwind CSS is a highly customizable, low-level CSS framework that gives you all of the building blocks you need to build bespoke designs without any annoying opinionated styles you have to fight to override.


![https://media.giphy.com/media/xT8qBsOjMOcdeGJIU8/giphy.gif](https://media.giphy.com/media/xT8qBsOjMOcdeGJIU8/giphy.gif)



## Appwrite Setup:

First we need to setup Appwrite to use its back-end features. There are two ways which you can do it. 

1. Docker ( Manual Configuration )
2. Digital Ocean droplets ( pre-configured )

I will be trying out both ways but , In this tutorial we will be going with the second way. 

*Note : There will be article on how to manually configure Appwrite using docker* 

*Pre-requisite : You need to have digitalocean account* 

Click on [this](https://marketplace.digitalocean.com/apps/appwrite)  link to create app-write droplet.

Once you get to the on-boarding screen, choose a basic plan ( Shared CPU ) and for CPU options we will go with minimum requirements. Then create a SSH key and keep it safe. Once the above things are done click on create a droplet. 

![description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/576zrpo9n7dwuv4ekozw.png)
  

Head over to this [link](https://cloud.digitalocean.com/droplet) and wait for sometime so that the docker images are installed in the droplet. 

![description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/g4v8xtmx8c72jjg8cznf.png)


Click on the **get started** link which will open a side navigation modal. Click on `Quick access to Appwrite` which will take you the Appwrite console. It will look something like this. 

*Note: If you have a custom domain you can also link it to the digital ocean droplet. Once that is done , Appwrite will automatically take care of installing the SSL certificates for that domain.*

![description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/mzd9ahyotc9bzzp2lr7o.png)
 
Now with console setup is done, you will have to create your account. 

Once you have created your account, click on `Create Project`. We will name the project as `Movie app`.

Now we need to create a platform. Click on `Add Platform` and click on `New Web App.`  Give the name of the **web-app** as `Movie-web` and hostname as `localhost`  and click on `Register.` 

Finally, the setup complete. 

![https://media.giphy.com/media/fUSrPZImoDEpSN9XIY/giphy.gif](https://media.giphy.com/media/fUSrPZImoDEpSN9XIY/giphy.gif)

Let's create collection and add bunch of attributes.

Click on the `database` in the navigation menu on the left side. Click on `Add Collection` and create a new collection called `Movies.`

![description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/sfcoqjqzhke73ciu74u9.png)
 

Now, head over to the collection and go to the `Attributes` tab and create the following `attributes`

![description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ndyv5u5s593cx0x6dmnj.png)
 
Level 1 complete i,.e now our back end powered by Appwrite is ready to accept some data.

Nice work so far 👏 👏 👏. 

![https://media.giphy.com/media/M8dw7l8jhQvSAVpawW/giphy.gif](https://media.giphy.com/media/M8dw7l8jhQvSAVpawW/giphy.gif)

Before setting up the front end , one small step we need to do that is getting the API key from the TMDB. 

Head over the **[TMDB](https://www.themoviedb.org/)** site and create your account. Once done go to this **[link](https://www.themoviedb.org/settings/api)** and create your API Key. Keep it safe , we will needing them when we code the API. 

## Front end setup

Head over to the terminal and run the following command. This will create a boilerplate app which will contain the basic Next.js and tailwindcss code. 

```bash
npx create-next-app --example with-tailwindcss nextjs-movie-appwrite

# install the deps
yarn install

# run the app locally
yarn dev
```

Now create your `.env.local` file at the root of your application and paste in the following

```bash
APPWRITE_ENDPOINT=https://[HOSTNAME_OR_IP]/v1
APPWRITE_PROJECT_ID=
APPWRITE_SERVER_API_KEY=
APPWRITE_COLLECTION_ID=
TMDB_MOVIE_KEY=
```

For `APPWRITE_ENDPOINT`, since we are using digital ocean’s pre-configured Appwrite droplet you will be seeing a IP address in your url bar. 

You can find the `APPWRITE_PROJECT_ID` in your console settings. 

![description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/s218rqtpt9ujvavnkl6h.png)
 

For getting the `COLLECTION_ID` , head over to `database/collections/settings` to find the collection id. 

![description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3q46vkdqddl5edxok1xu.png)
 

We will be using Appwrite’s Server side SDK for our application. Let’s first install the dependency

Go to the terminal and type in the following command 

```bash
yarn add node-appwrite axios uuid
```

## Seeding the database:

Let’s create library function which will initialise the `Appwrite` SDK. 

Create a new file called `appwrite.ts` under the folder `lib` and paste in the following code

```javascript

import Appwrite from 'node-appwrite'

export const initAppwrite = () => {
  const sdk = new Appwrite.Client()
  sdk
    .setEndpoint(process.env.APPWRITE_ENDPOINT as string) // Your API Endpoint
    .setProject(process.env.APPWRITE_PROJECT_ID as string) // Your project ID;
    .setKey(process.env.APPWRITE_SERVER_API_KEY as string)

  return sdk
}


```
**How do you get the Server API Key ?**

Go to the `Appwrite console` and click on the **API keys**

![description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8fp6ai18jyddo6z3n2uk.png)

Create a new API key with the following scopes 

```jsx
collections.read
collections.write
document.read
document.write
```

![description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/48r8jiizc3rvf3armvst.png)

 
Go to `pages/api` and create a new file `seed.ts` and add in the following code

```jsx
// Next.js API route support: https://nextjs.org/docs/api-routes/introduction


import type { NextApiRequest, NextApiResponse } from 'next'
import Appwrite from 'node-appwrite'
import axios from 'axios'
import { v4 as uuidv4 } from 'uuid'
import { initAppwrite } from '../../lib/appwrite'

type Data = {
  data: string
}

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse<Data>
) {
  let sdk = initAppwrite()
  let database = new Appwrite.Database(sdk)
  const response = await axios(
    `https://api.themoviedb.org/3/discover/movie?api_key=${process.env.TMDB_MOVIE_KEY}&language=en-US&sort_by=popularity.desc&include_adult=false&include_video=false&page=${req.query.page}`
  )
  const data = await response.data

  try {
    for (let i = 0; i < data.results.length; i++) {
      const item = data.results[i]
     let promise = await database.createDocument(
        process.env.APPWRITE_COLLECTION_ID as string,
        uuidv4(),
        {
          movie_id: item.id,
          title: item.title,
          thumbnail_image: `https://image.tmdb.org/t/p/original/${item.poster_path}`,
          popularity: item.popularity,
          release_date: item.release_date,
          vote_average: item.vote_average,
        }
      )

      console.log(promise)
    }
    res.status(200).json({ data: 'OK' })
  } catch (e) {
    console.log(e)
    res.status(500).json({ data: 'NOT OK' })
  }
}
```


*Code walkthrough:*

First, we initialise the **sdk** with help of the library function `initAppwrite`  which gives us the instance of the sdk which we can use to create collections , documents etc.

In the API,  we will get the page number as the query param which will be used, when fetching the movies from the **TMDB API**. 

Once we get the response , we loop through movie results and create a document using `database.createDocument` method provided by `Appwrite`.

`**database.createDocument**` will take 3 arguments which is collection id , document id ( which we generate with help of the uuid package to give us a unique id for each document ) and finally the details for each document. 


Now head over to the browser and type in the following url. *Make sure your server is running.* 

```bash
http://localhost:4040/api/seed?page=10
```

If everything goes as expected , you should see a JSON response `OK` on your browser, alternatively you can head over to the Appwrite console and go to the collection to check if those documents are added.

Let’s do that. 

Your browser should show something like this

![adding to the appwrite database as a ](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/14b9ttuix5y84f79ckq1.png)
  
![description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/z6erhgo96m489xx2tw7s.png)

Now our DB is successfully seeded with bunch of movie data. 
 

let's get those movie from the database and display it on our application's homepage 


## Rendering movies in the Home page:

Let's create our layout component, so that every time we create our page we don't have to rewrite the same thing over and over again. 

Create a components folder, inside that create `layout.tsx` file and paste in the following content.

```javascript

import Link from 'next/link'

const Layout = ({ children }: any) => {
  return (
    <>
      <header className="body-font text-gray-600">
        <div className="container mx-auto flex flex-col flex-wrap items-center p-8 md:flex-row">
          <Link href="/">
            <span className="title-font mb-4 flex cursor-pointer items-center text-2xl font-medium text-gray-900 md:mb-0">
              Mflix
            </span>
          </Link>
          <nav className="flex flex-wrap items-center justify-center text-base md:mr-auto	md:ml-4 md:border-l md:border-gray-400 md:py-1 md:pl-4">
            <span className="mr-5 hover:text-gray-900">
              <Link href="/add">Add Movie</Link>
            </span>
            <span className="mr-5 hover:text-gray-900">
              <Link href="/bookmark">Bookmarks</Link>
            </span>
          </nav>
        </div>
      </header>
      <div className="container mx-auto p-8">{children}</div>
    </>
  )
}

export default Layout

```

Head over to your browser to see the layout changes. 

![description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/c6u3mknpijthi09ix5kx.png)


We will create a API endpoint called `getMovies` and use them to fetch the data from the database.

In the `API` folder , create a file called `getMovies.ts` and add in the following content.

```javascript

import { NextApiRequest, NextApiResponse } from 'next/types'
import Appwrite from 'node-appwrite'
import { initAppwrite } from '../../lib/appwrite';


export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {

    try{
        let sdk = initAppwrite();
        let database = new Appwrite.Database(sdk)
        
        const limit:any = req.query.limit;
        const offset:any = req.query.offset;

        let response = await database.listDocuments(process.env.APPWRITE_COLLECTION_ID as string, [],limit, offset)
        console.log(response.total)
        res.status(200).json({ data: response, count: response.total })
      
    }catch(e){
        res.status(500).json({data: e})
    }   

  
}


```

**Code Walkthrough**

In the above code we made use of the `[listDocuments](https://appwrite.io/docs/server/database?sdk=nodejs-default#databaseListDocuments)` method provided by `Appwrite`.

We will be able to get 25 documents by default from the database, which is why we will be passing the values of limit and offset as query parameter while calling the API. 

**What is Offset ?**

Offset in this context is basically when you tell the Appwrite server to skip the number of records before fetching. 

That did not make sense right.

![https://media.giphy.com/media/WRv4LT6GmtiABVpYNp/giphy.gif](https://media.giphy.com/media/WRv4LT6GmtiABVpYNp/giphy.gif)


For example, Let's say you want to go to page 2 , now you don't need to have the first 25 records , so you can mention in the offset param as 25 which will skip the first 25 records and get you the next 25 records to be displayed on the interface.


Let's write our UI code to call the API when the component mounts. In the `pages` folder create a file named `index.tsx` and add in the following content. 


```javascript

// index.tsx

import type { NextPage } from 'next'
import Link from 'next/link'
import { useEffect, useState } from 'react'
import Layout from '../components/layout'
import Pagination from '../components/pagination'


const Home: NextPage = () => {
  
  const [offset, setOffset] = useState(0);
  const [pageLimit] = useState(25)
  const [totalPageCount, setTotalPageCount] = useState(0)

  const fetchMovies = async (limit: any,offset: any) =>{
    const response = await fetch(`/api/getMovies?limit=${limit}&offset=${offset}`)
    const data = await response.json()
    setMovies(data.data.documents)
    setTotalPageCount(data.count)
  }

  useEffect(()=>{
    fetchMovies(pageLimit, offset)
  },[offset])

  const handleNextPage =  () => {
    setOffset((prevOffset) => prevOffset + pageLimit)
  }

  const handlePreviousPage = () => {
   if(offset > 0 ){
    setOffset((prevOffset) => {
      return prevOffset - pageLimit
    })
   } 
  }
  const [movies, setMovies] = useState([])

  return (
    <Layout>
      <section className="body-font text-gray-600 pb-4">
        <div className="mt-6 grid grid-cols-1 gap-16 sm:grid-cols-2 lg:grid-cols-4 xl:gap-x-20">
          {movies.map((movie: any) => {
            return (
              <a className="cursor-pointer">
                <Link href={`/movie/${movie.movie_id}`}>
                  <div key={movie.movie_id} className="group relative">
                    <div className="min-h-80 aspect-w-1 aspect-h-1 lg:aspect-none w-full overflow-hidden rounded-md bg-gray-200 group-hover:opacity-75 lg:h-80">
                      <img
                        src={movie.thumbnail_image}
                        alt={movie.title}
                        className="h-full w-full object-cover object-center lg:h-full lg:w-full"
                      />
                    </div>
                    <div className="mt-4 flex justify-between">
                      <div>
                        <h3 className="text-gray-700">
                        
                          {movie.title}
                        </h3>
                        <p className="mt-1 text-sm text-gray-500">
                          {new Date(movie.release_date).toDateString()}
                        </p>
                      </div>
                      <p className="text-sm font-medium text-gray-900">
                        {movie.price}
                      </p>
                    </div>
                  </div>
                </Link>
              </a>
            )
          })}
        </div>
      </section>
      <Pagination 
        totalMovies={totalPageCount}
        postPerPage={pageLimit}
        nextOffset={offset + pageLimit}
        currentPage={0}
        handleNextPage={handleNextPage}
        handlePreviousPage={handlePreviousPage}
      />
    </Layout>
  )
}

export default Home

```


*Code walkthrough*


First when the component mounts , we set three local variables offset, page limit and  page count. Then the useEffect runs fetching the movies and setting up the movie data.

we use the same data to render them in the screen.

Note: I have used useEffect to do a data fetching in the client side. Alternatively , you can also do a server side data fetching using `getServerSideProps`. 

In the pagination component we use the relevant props to navigate between the pages.

Inside the components folder, create a new file called `Pagination.tsx` and add in the following content.


```javascript



const Pagination = (props: {
  totalMovies: number
  postPerPage: number
  currentPage: number
  handleNextPage: () => void
  nextOffset: number
  handlePreviousPage:  () => void
}) => {
  return (
    <div className="flex items-center justify-between border-t border-gray-200 bg-white px-4 py-3 sm:px-6">
      <div className="flex flex-1 justify-between">
        <button
          disabled={props.currentPage === 1}
          onClick={props.handlePreviousPage}
          className="relative inline-flex items-center rounded-md border border-gray-300 bg-white px-4 py-2 text-sm font-medium text-gray-700 hover:bg-gray-50"
        >
          Previous
        </button>

        {props.nextOffset <= props.totalMovies && (
          <button
            onClick={props.handleNextPage}
            className="relative ml-3 inline-flex items-center rounded-md border border-gray-300 bg-white px-4 py-2 text-sm font-medium text-gray-700 hover:bg-gray-50"
          >
            Next
          </button>
        )}
      </div>
    </div>
  )
}

export default Pagination

      
```


*Code walkthrough*

In the pagination component, `handlePreviousPage` and `handleNextPage` will take care of the navigating between pages ( takes care of setting up the offset value accordingly ) and then we disable `prev` and `next` button based on the offset value. 

Head over to the browser to see the magic 


![description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/f39c7gzhshotxaicr8s4.png)
 


## Adding movies to the Database via form :


We are going to add the movie to the collection via a form from the browser. 

Fun part is, we won't have to add to movie name , year or upload the image. TMDB API has a way to fetch the information using `IMDB id`.

The IMDB id in the url **https://www.imdb.com/title/tt4154756/?ref_=hm_tpks_tt_i_2_pd_tp1_cp** is **tt4154756**. Once we input the `ID` in the form , it will fetch relevant movie information and check if it is already in the database , if not it will add them to the database. 


Let's see how we can do that. First as usual ,let's create our API endpoint.

Go to `api` folder and create a new file called `addMovies.ts` and add in the following content

```javascript

import type { NextApiRequest, NextApiResponse } from 'next'
import { initAppwrite } from '../../lib/appwrite'
import Appwrite, { Query } from 'node-appwrite'
import { v4 as uuidv4 } from 'uuid'

type Data = {
  data?: any,
  id?: number
}
export default async (req: NextApiRequest, res: NextApiResponse<Data>) => {
  try {
    const imdbID = req.query.imdbID
    const sdk = initAppwrite()
    let database = new Appwrite.Database(sdk)

    const response = await fetch(
      `https://api.themoviedb.org/3/find/${imdbID}?api_key=${process.env.TMDB_MOVIE_KEY}&language=en-US&external_source=imdb_id`
    )
    const data = await response.json()

    if (data.movie_results.length > 0) {

      const id = data.movie_results[0].id
      const item = data.movie_results[0]

      const movieInDatabase = await database.listDocuments(
        process.env.APPWRITE_COLLECTION_ID as string,
        [Query.equal('movie_id', id)]
      )

      if (movieInDatabase.documents.length > 0) {
        // there are movies present in the database;
        return res.status(409).json({ data: 'Movie already exists', id })
      } else {
        // add the movie to the database
        await database.createDocument(
          process.env.APPWRITE_COLLECTION_ID as string,
          uuidv4(),
          {
            movie_id: id,
            title: item.title,
            thumbnail_image: `https://image.tmdb.org/t/p/original/${item.poster_path}`,
            popularity: item.popularity,
            release_date: item.release_date,
            vote_average: item.vote_average,
          }
        )

        return res.status(200).json({id })
      }
    }else{
        console.log("it is not a movie")
    }
  } catch (e) {
    console.log(e)
    res.status(500).json({ data: e })
  }
}

```


As usual , we initialise the Appwrite SDK and then fetch the movie details using TMDB api. We check if it is a TV show related **IMDB id** has been entered and then we check if the document is present in the collection. 

If the document is found, then we send in the response saying movie already exists. If it is not found then we add it to the collection. 


Now onto coding the component. Create a file called `add.tsx` in the pages directory and add in the following content


```javascript

import { useRouter } from 'next/router'
import { useState } from 'react'
import Layout from '../components/layout'

const Add = () => {
  const [id, setImdbId] = useState('')
  const router = useRouter()
  const handleAddMovie = async () => {
    try {
      const response = await fetch(
        `/api/addMovie?imdbID=${id}`
      )
      const data = await response.json()
      if (response.status === 200 && data.id) {
        console.log(
          'Added the movie succesfully, redirecting to the movie page'
        )
        return router.push(`/movie/${data.id}`)
      }
      if(response.status === 409 && data.id){
        console.log(
            'Movie already exists, redirecting to the movie page'
          )
          return router.push(`/movie/${data.id}`)
      }
    } catch (e) {
      console.log(e)
    }
  }

  return (
    <Layout>
      <label className="flex flex-row space-x-2">
        <input
          type="text"
          className=" block w-1/2"
          placeholder="Enter IMDB ID"
          onChange={(e) => setImdbId(e.target.value)}
        />
        <button
          className="bg-black p-2 px-4 text-lg text-white"
          onClick={handleAddMovie}
        >
          Add Movie
        </button>
      </label>
    </Layout>
  )
}

export default Add


```

In the component we get the `IMDB id` from the user and then we sent it to the API. 


Based on the response we get from the API we create a movie and redirect it to separate movie page ( which we will be coding in the later part of this tutorial ).

![description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tmxvhwa33sgxt3215pne.png)
 

## Bookmarking the movie:

Let's code our last feature for the application, which is bookmarking the movie. We will first create our separate movie page and add in the bookmark. We will be using [Next.js dynamic routes](https://nextjs.org/docs/routing/dynamic-routes) feature for this. 

Create a folder called **movie** in the pages directory and inside that create a file called `[id]`.tsx.

Now if we go to the url `localhost:3000/movie/1` we can send the id present in the url to the API to get the particular movie detail. 

Inside the `[id].tsx` file you created , add in the following content.

**Note**: We will be storing the bookmarked movies in the IndexDB. For that I am using a package called **[localforage](https://localforage.github.io/localForage/)**. 

Run this command to install the dependency 

`yarn add localforage`



```javascript

import axios from 'axios'
import localforage from 'localforage'
import { useEffect, useState } from 'react'
import Layout from '../../components/layout'

const Movie = (props: { movie: any }) => {
  const { movie } = props
  const [bookmarkStatus, setBookMarkedStatus] = useState(false);

  useEffect(()=>{
    const setBookMarkOnInit = async () => {
      const movies:any = await localforage.getItem('movies');
      if(movies && movies.length === 0){
        const filterMovie = movies.filter((data: any) => data.movie_id === movie.id);
        console.log(filterMovie)
        if(filterMovie.length  > 0){
          setBookMarkedStatus(true)
        }
      }
     
    }
    setBookMarkOnInit()
  },[])

  const handleBookmark = async () => {
    console.log("store the data in indexDB")
    const data = await localforage.getItem("movies");

    const movieDataToStore = {
      movie_id: movie.id,
      title: movie.title,
      thumbnail_image: `https://image.tmdb.org/t/p/original/${movie.poster_path}`,
      popularity: movie.popularity,
      release_date: movie.release_date,
      vote_average: movie.vote_average,
    }
    if(!data){
      localforage.setItem("movies", [movieDataToStore])
      setBookMarkedStatus(true)
    }else{
      const existingData:any = await localforage.getItem('movies');
      const filteredData:any = existingData.filter((data: { movie_id: any }) => movie.id === data.movie_id);
      console.log("movie exists" ,filteredData)
      if(filteredData.length === 0){
        // movie does not exists in the indexDB 
        localforage.setItem("movies", [...existingData, movieDataToStore])
        setBookMarkedStatus(!bookmarkStatus)
      }
    }
  }


  return (
    <>
      <Layout>
        <div className="flex flex-col space-y-8 items-center lg:items-start lg:flex-row lg:space-x-8 lg:space-y-0">
          <img
            src={`https://image.tmdb.org/t/p/original/${movie.poster_path}`}
            className="w-2/3 lg:w-1/4"
          />
          <div className="flex flex-col justify-items-start space-y-12">
            <div className="flex flex-col  items-center justify-between space-y-2 lg:space-y-4 lg:items-start">
              <h1 className="text-2xl font-bold text-center lg:text-left"> {movie.title} </h1>
              <div className="flex flex-shrink-1 items-center  pr-2 text-sm space-x-2">
                <div className="rounded-r bg-green-200 px-2 py-1 text-green-700">
                  <p> {movie.status} </p>
                </div>
                {movie.genres.map((genre: any) => {
                  return (
                    <div className="rounded-r bg-green-200 px-2 py-1 text-green-700 whitespace-nowrap">
                      <p> {genre.name} </p>
                    </div>
                  )
                })}
              </div>
            </div>
            <div className='text-center md:text-justify'>
              <p> {movie.overview} </p>
            </div>
            <div className="flex flex-col space-y-2 md:flex-row md:space-x-2 md:space-y-0">
              <a
                href={movie.homepage}
                target="_blank"
                className="flex items-center space-x-2   bg-black p-2 pl-5 pr-5 text-lg text-white "
              >
                <svg
                  className="h-6 w-6"
                  fill="currentColor"
                  viewBox="0 0 20 20"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path d="M11 3a1 1 0 100 2h2.586l-6.293 6.293a1 1 0 101.414 1.414L15 6.414V9a1 1 0 102 0V4a1 1 0 00-1-1h-5z"></path>
                  <path d="M5 5a2 2 0 00-2 2v8a2 2 0 002 2h8a2 2 0 002-2v-3a1 1 0 10-2 0v3H5V7h3a1 1 0 000-2H5z"></path>
                </svg>
                <span>Movie Homepage</span>
              </a>
              <button className='flex items-center space-x-2   bg-black p-2 pl-5 pr-5 text-lg text-white ' onClick={handleBookmark}>
              <svg className="w-6 h-6" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path d="M5 4a2 2 0 012-2h6a2 2 0 012 2v14l-5-2.5L5 18V4z"></path></svg>
              <span> {bookmarkStatus ? "Bookmarked" : "Bookmark"} </span>
              </button>
            </div>
          </div>
        </div>
      </Layout>
    </>
  )
}

export async function getServerSideProps(context: any) {
  const { id } = context.query

  const response = await axios(
    `https://api.themoviedb.org/3/movie/${id}?api_key=${process.env.TMDB_MOVIE_KEY}&language=en-US`
  )

  const data = await response.data
  return {
    props: {
      movie: data,
    },
  }
}

export default Movie


```

Since we are storing the movie id in our database , we pass it dynamically when the user visits the separate movie page. We use that in the `getServerSideprops` method to fetch the particular movie data. 

Since we are not storing the particular movie data in the database , we use the TMDB API to fetch the movie information. 

When the component mounts, we need to check whether the movie has already been bookmarked or not. For that we use `useEffect` combined with `localforage` method to get the bookmarked status. 

When the user clicks on the `Bookmark button` , we then add the movie to localstorage list. 

When you visit your browser , you should see something like this

![description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kr6y97nzpyj0on6i5o5m.png)
 
![description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fs4reunv5w4rya7tsnnx.png)


Now that we can bookmark the movies, we need a place to render them. Let's quickly create a page named `bookmark.tsx` and add in the following content.


```javascript

import Layout from '../components/layout'
import { useEffect, useState } from 'react'
import localforage from 'localforage'
import Link from 'next/link'

const Bookmark = () => {
  const [bookmarkedMovies, setBookMarkedMovies] = useState([])
  useEffect(() => {
    const getMovieFromLocalStorage = async () => {
      const bookMarkedMovies: any = await localforage.getItem('movies')
      if (bookMarkedMovies && bookMarkedMovies.length !== 0) {
        setBookMarkedMovies(bookMarkedMovies)
      }
    }
    getMovieFromLocalStorage()
  })

  const renderMovies = () =>{
    if(bookmarkedMovies.length === 0){
      return(
        <> 
          <p> No Bookmarked movies found. Head over to <Link href={'/'}> Movies </Link></p>
        </>
      )
    }
    return(
      bookmarkedMovies.map((movie: any) => {
        return (
          <a className="cursor-pointer">
            <Link href={`/movie/${movie.movie_id}`}>
              <div key={movie.movie_id} className="group relative">
                <div className="min-h-80 aspect-w-1 aspect-h-1 lg:aspect-none w-full overflow-hidden rounded-md bg-gray-200 group-hover:opacity-75 lg:h-80">
                  <img
                    src={movie.thumbnail_image}
                    alt={movie.title}
                    className="h-full w-full object-cover object-center lg:h-full lg:w-full"
                  />
                </div>
                <div className="mt-4 flex justify-between">
                  <div>
                    <h3 className="text-gray-700">{movie.title}</h3>
                    <p className="mt-1 text-sm text-gray-500">
                      {new Date(movie.release_date).toDateString()}
                    </p>
                  </div>
                  <p className="text-sm font-medium text-gray-900">
                    {movie.price}
                  </p>
                </div>
              </div>
            </Link>
          </a>
        )
      })
    )
  }
  return (
    <Layout>
      <section className="body-font text-gray-600">
        <div className="mt-6 grid grid-cols-1 gap-16 sm:grid-cols-2 lg:grid-cols-4 xl:gap-x-20">
          {renderMovies()}
        </div>
      </section>
    </Layout>
  )
}

export default Bookmark


```

In this component , we simply fetch the data from indexDB and render it on the screen. 


You should see something like this in your browser.

![description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/r87cdv84ofk0wbi24g2j.png)
 
 That's it. You can deploy the finished code to vercel. 

![https://media.giphy.com/media/OoTKFwKiOAbYc/giphy.gif](https://media.giphy.com/media/OoTKFwKiOAbYc/giphy.gif)


## Conclusion


That's pretty much it. Thank you for taking the time to read the tutorial. I hope , everyone understood how to create application using Appwrite and Nextjs. 


If you found the post useful , add ❤️ to it and let me know if I have missed something or if you have any doubts in the code in the comments section. 

Feedback on the tutorial is most welcome.


**Repository Link**: [App write Movie](https://github.com/skarthikeyan96/appwrite-nextjs-movie)

**Deployed Link**: [appwrite-nextjs-movie.vercel.app](https://appwrite-nextjs-movie.vercel.app/)

**Digital Ocean Free $100 credits** :(Not a compulsion to use this referral code )

[![DigitalOcean Referral Badge](https://web-platforms.sfo2.cdn.digitaloceanspaces.com/WWW/Badge%201.svg)](https://www.digitalocean.com/?refcode=9323dde7dc7f&utm_campaign=Referral_Invite&utm_medium=Referral_Program&utm_source=badge)


Social Links:

[Twitter](https://twitter.com/karthik_coder)
[Showwcase](https://www.showwcase.com/karthik-codes)