## Getting started with Deno

In this blog post , we will learn about denojs and  create a simple CLI application that gives the current weather information.

## Outline 

```
1. What is Deno ? 
2. Features of Deno.
3. Installing Deno in local environment. 
4. Building Weather App CLI .
5. Conclusion. 
```

## What is Deno ?

Deno is a secure runtime for Typescript and Javascript written on Rust. Deno programs can be written with either Javascript or Typescript and executed from command line. 

The concept of Deno was first revealed to the world during JSConf EU 2018, where Ryan Dahl gave a talk on “10 Things I Regret About NodeJS”. You can find recording of the talk  [here](https://www.youtube.com/watch?v=M3BM9TB-8yA).

It is secure and browser compatible as possible. 

## Features 

1. Secure by default. No file, network, or environment access (unless explicitly enabled).
2. Supports TypeScript out of the box.
3. Compatibility with web as possible and supports web api's like fetch.
4. Provide built-in tooling like test runner , formatter and linter to improve developer experience.


Now onto the fun part , let's build our Deno Weather CLI 

## Installing in Deno in local environment 

Deno works on macOS, Linux, and Windows. Deno is a single binary executable. It has no external dependencies.

On macOS, both M1 (arm64) and Intel (x64) executables are provided. On Linux and Windows, only x64 is supported.

Fire up your terminal and run this command. 

```shell
# For Mac OS using curl 

curl -fsSL https://deno.land/x/install/install.sh | sh

```

```shell

# For Windows using power shell

iwr https://deno.land/x/install/install.ps1 -useb | iex

```

Once successfully  installed , you should see something like this 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632159511060/pnF4CDoL0.png)

Run the following command to verify the deno installation.

```shell

deno --version

```

Installation docs for deno from other source  -- [Link to docs](https://deno.land/manual@v1.14.0/getting_started/installation)

## Building a Weather App CLI

We will be coding our CLI in JS. 

You can also install official VS code plugin for Deno , if you are not using VScode, deno's [docs](https://deno.land/manual@v1.14.0/getting_started/setup_your_environment) has ways to setup the dev environment, prior to coding. 



Let’s create a folder for our new project  and add an index.js file:

```shell

mkdir weather-app-cli-deno
cd weather-app-cli-deno
code-insiders index.js

```

We are going to get the weather data from the [openweather map](https://openweathermap.org/) API. 

Once you create your account , you should be able to create your API key in this [link](https://home.openweathermap.org/api_keys).

Alright now, keep it safe. We will be needing it to query the data. 

Open up your `index.js` file and paste the following code. 


```javascript

console.log(Deno.args) // [ "London" ]

let city = Deno.args[0]; // Deno.args gives us a way to access the CLI variables

if(!city){
    console.error("No city supplied");
    Deno.exit();
}

const apiKey = '*****************'; // replace with your API key

const res = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${apiKey}`);
const data = await res.json();

console.log(data);


```

Now if you run this above code with following command

```shell

deno run index.js London

```

You will get the following output 

```shell

error: Uncaught PermissionDenied: Requires net access to "api.openweathermap.org", run again with the --allow-net flag
const res = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${apiKey}`);
                  ^
    at deno:core/01_core.js:106:46
    at unwrapOpResult (deno:core/01_core.js:126:13)
    at Object.opSync (deno:core/01_core.js:140:12)
    at opFetch (deno:ext/fetch/26_fetch.js:57:17)
    at mainFetch (deno:ext/fetch/26_fetch.js:199:61)
    at deno:ext/fetch/26_fetch.js:439:11
    at new Promise (<anonymous>)
    at fetch (deno:ext/fetch/26_fetch.js:399:15)
    at file:///Users/karthikeyan/karthikeyan/weather-app-cli-deno/index.js:11:19

```

The reason is because , deno being secure runtime which will have access network unless we give access. As mentioned in the error message, let's give access to the network and see what happens.

tweak the command like this and run it in your terminal 

```shell

deno run --allow-net index.js Bangalore

```

We will successfully get the current weather information about Bangalore.

```shell

{
  coord: { lon: 77.6033, lat: 12.9762 },
  weather: [
    {
      id: 200,
      main: "Thunderstorm",
      description: "thunderstorm with light rain",
      icon: "11n"
    },
    { id: 502, main: "Rain", description: "heavy intensity rain", icon: "10n" }
  ],
  base: "stations",
  main: {
    temp: 20.8,
    feels_like: 21.55,
    temp_min: 20.8,
    temp_max: 21.9,
    pressure: 1014,
    humidity: 100
  },
  visibility: 4000,
  wind: { speed: 0, deg: 0 },
  rain: { "1h": 8.65 },
  clouds: { all: 75 },
  dt: 1632161523,
  sys: { type: 1, id: 9205, country: "IN", sunrise: 1632098322, sunset: 1632142053 },
  timezone: 19800,
  id: 1277333,
  name: "Bengaluru",
  cod: 200
}

```

Let's pick what we need from the response , format it and display it in the terminal.

Replace the following formatted code with old code. 

```javascript

console.log(Deno.args,'\n')
const city = Deno.args[0]; 

if(!city){
    console.error("No city supplied");
    Deno.exit();
}

const apiKey = '******************************';

const res = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${apiKey}`);
const data = await res.json();


const date = new Date(data.dt * 1000).toLocaleDateString()

console.log("========== Weather Report ============\n")
console.log(`Date - ${date} | Location: ${data.name}`)
console.log("\n")
console.log(`Weather- ${data.weather[0].description} | Temparature: ${data.main.temp} \n`)


```

and run the following command to see the output again.

```shell

 deno run --allow-net index.js Bangalore


```

You should see the following output on the terminal


```shell


======== Weather Report ============

Date - 20/09/2021 | Location: Bengaluru


Weather- thunderstorm with light rain | Temparature: 20.8 


```

Congratulations 👏 👏 👏 . You have built your first CLI  using Deno.


## Conclusion

Thanks for reading till the end. Let me know your thoughts in the comments. Feedbacks are always welcome.


Resources:

1. [Deno Docs](https://deno.land/manual@v1.14.0/getting_started)


Stay Safe and Happy coding :)


