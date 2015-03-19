You can think of [Keen.io](http://keen.io) as Software-as-a-Service for analytics: you pass them data, they store it for you, and you query the data using their API. They keep the data layer, backend servers & hardware, and infrastructure running, so you can focus on the numbers as a software developer or data scientist.

![Keen.io](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/00.png)

## Sign Up

It's free to store data in Keen.io for up to 50,000 events. That is plenty of storage to play around with their service and test it out for your needs. The first step is to sign up for an account from [their signup page](https://keen.io/signup).

After signing up, you will see a New Project button. Click it to setup your first project. A Project is a separate database to store your project's data.

![New Project Button](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/01.png)

![Creating New Project](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/02.png)

After creating a new project, you'll be sent to the Project Overview screen which contains some basic data about your project.

![Project Overview](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/03.png)

The upper left block of the screen contains the name of your project that you used when signing up, and a Project ID. The Project ID is used to reference this specific project when calling the Keen.io API to retrieve your data.

![Show API Keys](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/04.png)

Clicking on Show API Keys shows you the Write Key, Read Key, and Master API Key. You'll use all of these specifically later on when calling the API along with your Project ID. Clicking Generate New Keys will generate new API Keys in the case your keys ever get compromised. It's always a good idea to keep these keys private, especially the Write Key and Master API Key!

![Event Explorer](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/05.png)

The Event Explorer contains a dropdown menu of events which have been collected. Clicking on the Last 10 Events or Event Properties buttons will tell you the respective information about the events you have collected so far. The Last 10 Events functionality is especially useful when first getting acquainted to the service.

Note that you'll also see a Delete Collection button, but this isn't generally used as [Keen.io considers deleting events a bad practice](https://keen.io/docs/maintenance/#delete-events-maintenance). It's also worthy to note that deleting collections are not immediate, and can take a while to complete.

## About Event Data

Before going any further, it's worth mentioning what Event Data actually is. It's important to know the difference between [Entity Data and Event Data](https://keen.io/blog/53958349217/analytics-for-hackers-how-to-think-about-event-data). It seems a bit silly saying this, but you can think about Event Data as anything tied to a specific event that is happening.

![Entity Data](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/06.png)

Here is an example of entity data. Entity data is the data that is generally thought about when thinking about a relational database, while event data is more more loosely structured and denormalized. 

![Event Data](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/07.png)

And here is an example of event data. All events contain an Action, a Timestamp, and a State. The Action is what is currently happening with the event, the Timestamp is the exact time it is happening, and the State is other information that is related to that event.

![Entity Data vs. Event Data](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/08.png)

Event data is schemaless (compared to entity data which is normalized & relational), and it can also contain much more information than is typically tied to a specific entity. The denormalized and schemaless nature of event data is necessary for scaling at a very high volume and for more efficeint data analysis.

## Why Keen.io?

There are many similar services out there that are akin to Keen.io, such as Google BigQuery, Segment, and Amazon RedShift (amongs MANY others). But there are a few things that really set Keen.io apart from the rest:

* **Ease of Use:** I have yet to find another analytical platform that has such a streamlined focus on getting event data into their system so quickly and easily. Other services like BigQuery and RedShift have great use-cases for more technical applications, however, I could not get up and running nearly as quickly on these platforms.
* **Software-as-a-Service:** Most of the analytical platforms out there are indeed SaaS-based, however others with very compelling similar technologies ([Druid](http://druid.io) comes to mind), are not SaaS-based. Not having to run my own data servers is an enormous benefit, in my time, and the overall scalability of an application.
* **Available SDK's:** JavaScript, iOS, Android, Java, Node, PHP, Python, Ruby, cURL, .NET, Scala, Parse. Show me another service with this many ways to access their API! It's unbelievably great knowing that I can develop apps in various technologies, and still have complete interoperability between frameworks and languages.

## Modeling Your Data

I won't explain much on how to model your data, because Keen.io has some pretty great documentation on all of that in their [Data Modeling Guide](https://keen.io/docs/event-data-modeling/event-data-intro/). 

## Workbench

Keen.io's workbench is the workhorse of their admin API. This is the toolset that is used to query and analyze your event data.

![Workbench](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/09.png)

* **Visualization**: Visual representation of the data you are analyzing
* **JSON**: The JSON response of the representation of the data you are analyzing
* **JavaScript**: An example JavaScript snippet used to make the same API request using their JavaScript SDK (useful for embedding charts on websites)
* **Query URL**: A basic URL that can be very useful when making programmatic calls to Keen.io's API.

## Creating events

### What To Event

I'm an avid coffee drinker, so I've decided I want to log every time I drink a cup of coffee. It's an event now, isn't it?

![Coffee](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/10.jpg)

I need to decide on a name of my event, so I think I'll use: `servingsOfCoffee`. I typically like to camelCase my event names, and all of my event data, but that's just me -- feel free to use underscores instead if that's what floats your boat.

Note that I didn't use cupsOfCoffee, sipsOfCoffee, or anything else specific to the serving of coffee because I want to encapsulate all coffee events and don't want to silo my event into an unnecessary specificity. I may be drinking my 8oz steaming cup of Joe, or my cold 6.5oz can of Starbucks DoubleShot Espresso -- so I need to really think of a good descriptive name for my event because I can't change it later. In this case, a serving can be anything coffee related, and can track 6.5oz, 8oz, big gulp 32oz, hot coffee, cold coffee... you get the idea.

Also, note how this differs from entity data. My serving of coffee is very denormalized and allows me to store just about any data related to my coffee drink. I can define other parameters such as whether it `hasSugar` or `hasCream`, if my coffee is `servedIn` a can or mug, or if the `brand` of coffee is Starbucks or Kopi Luwak. I may even decide to add any/all of these, or other properties in the future - the benefit of using a schemaless design allows me to do this.

> **Note:**
> If you instead wanted to create an event called servingsOfDrinks to store all drinks consumed instead of coffee, you can do that. You can also make it so it logs all drinks consumed by everyone by passing in a unique identifier for each user tied to each drink serving. The sky is the limit, and it depends what you want to log and what you want to do with that data.

I decided to pick a `servingsOfCoffee` event just to show how arbitrary an event can be. We can store **ANY** event data that we wish. Some more examples: what I ate for dinner, how many miles I drive each day, how many customer registrations happen on my website, how long specific light bulbs are on each day... the list never ends. We are entering the world of [Internet of Things (IoT)](http://en.wikipedia.org/wiki/Internet_of_Things), and we will soon be logging any and every event on the planet, and beyond.

### Where To Event

We can use any of Keen.io's built-in methods to send data. Keen.io really has a great set of SDK's we can choose from to best suit our project or skillset. I'm a PHP and Node.JS guy, but for ease of use I'll be using the cURL method in this tutorial, because it really is the simplest to get going and has just about no prerequisites.

![SDKs](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/11.png)

Open up command line (or Terminal on Mac), and type curl and hit enter. If you get the response `curl: try 'curl --help' or 'curl --manual' for more information` that means you have cURL installed. If you don't have it installed, you can [download cURL](http://curl.haxx.se/download.html).

All cURL URL's will start with a format as follows:
`https://api.keen.io/3.0/projects/<PROJECT_ID>/events/<EVENT_COLLECTION>?api_key=<READ_OR_WRITE_KEY>`

Remember the Project ID and API Keys that were provided to us from our Project Overview page? We'll plug those into the URL above to start formatting our URL. Note that our URL may get a little unwieldy at times, however this is still the simplest method to demonstrate this tutorial, and as long as you break down the URL into different sections you should be able to follow along fairly easily.

In this case, the base URL that we will use will be:
`https://api.keen.io/3.0/projects/5509ba1f59949a13de470f54/events/servingsOfCoffee?api_key=D499FEABD15DB6383BBBFBAA735FD5C0`

Note how we just subbed in the Project ID, name of the Event we'll be creating, and our API Key. For the purposes of this demo, we'll just use the Master API Key as it's much shorter and easier to read than the Read and Write Keys. For your project you will almost always want to instead use your Read or Write Keys instead of the Master API Key, as the Master can be used to delete data.

### How To Event

#### Create an Event

Next we will create a JSON file which contains our event, and we'll put this in a file named `sludge1.json`.
```
{
    "name": "Sludge",
    "numberOfOunces": 8,
    "hasCream": true,
    "hasSugar": true,
    "servedIn": "coffee mug"
}
```
We'll take the URL we just generated above, and put it into a format that cURL will understand.

`curl "https://api.keen.io/3.0/projects/5509ba1f59949a13de470f54/events/servingsOfCoffee?api_key=D499FEABD15DB6383BBBFBAA735FD5C0" -H "Content-Type: application/json" -d @sludge1.json`

This basically just passes the sludge1.json data to the URL, and tells cURL that we are sending JSON data. Just paste the above line in Terminal, hit enter, and you should receive the following response: `{"created": true}`

This tells us everything was posted successfully to Keen.io.

![Congrats](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/12.gif)

If you received any other response, you'll have to debug things to find out where things went wrong, but there really isn't much to go wrong at this point.

From here, we can refresh our Project Overview page and look at our Event Explorer to make sure our event showed up (note that it may take up to 10 seconds between your "created" response from Keen.io and it actually showing up in Keen.io's dataset). Low and behold, we see our servingsOfCoffee event.

![Project Overview](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/13.png)

If we select it, and click Last 10 Events, we will see our event data that we just passed in:

![Last 10 Events](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/14.png)

And details on the event:

![Event Detail](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/15.png)

Note that a keen property was added by Keen.io. This property contains a secondary object that contains the timestamp of when our event happened, when the record was created, and a unique identifier for the event. If you were loading in history data or wanted to define an exact date for the event, you can pass in your own datetime in ISO-8601 format for the property `keen.timestamp`. You can find more info on that at Keen.io's [Data Modeling Guide](https://keen.io/docs/event-data-modeling/event-data-intro/#id9).

#### Create More Events

Let's create a few more events, as it's a coffee day. Create each one of these as a separate JSON file, loading them in the same format as above:

`coffee1.json`
```
{
    "name": "Pike Place",
    "brand": "Starbucks",
    "numberOfOunces": 12, 
    "servedIn": "paper cup"
}
```
`coffee2.json`
```
{
    "name": "Light Roast",
    "brand": "Starbucks",
    "numberOfOunces": 8,
    "servedIn": "paper cup"
}
```
`coffee3.json`
```
{
    "name": "Hazelnut",
    "brand": "Seattle's Best",
    "numberOfOunces": 8,
    "servedIn": "coffee mug",
    "hasCream": false,
    "hasSugar": false
}
```
`coffee4.json`
```
{
    "name": "Kopi Luwak",
    "numberOfOunces": 4,
    "servedIn": "golden goblet"
}
```
Then run the cURL posts:

`curl "https://api.keen.io/3.0/projects/5509ba1f59949a13de470f54/events/servingsOfCoffee?api_key=D499FEABD15DB6383BBBFBAA735FD5C0" -H "Content-Type: application/json" -d @coffee1.json`

`curl "https://api.keen.io/3.0/projects/5509ba1f59949a13de470f54/events/servingsOfCoffee?api_key=D499FEABD15DB6383BBBFBAA735FD5C0" -H "Content-Type: application/json" -d @coffee2.json`

`curl "https://api.keen.io/3.0/projects/5509ba1f59949a13de470f54/events/servingsOfCoffee?api_key=D499FEABD15DB6383BBBFBAA735FD5C0" -H "Content-Type: application/json" -d @coffee3.json`

`curl "https://api.keen.io/3.0/projects/5509ba1f59949a13de470f54/events/servingsOfCoffee?api_key=D499FEABD15DB6383BBBFBAA735FD5C0" -H "Content-Type: application/json" -d @coffee4.json`

Let's now refresh our event collection, and we should now see five servingsOfCoffee events:

![5 servingsOfCoffee Events](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/16.png)

We can also click Event Properties to see how Keen.io sees the data we are loading in.

![servingsOfCoffee Event Properties](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/17.png)

This is useful to make sure numbers are imported as numbers, and strings are imported as strings. For example, to properly sum event property values, those properties need to be valid numbers (num) in JSON -- that means without double-quotes (which Keen.io assumes are strings).

## Visualize your events

Let's head back to our Workbench so we can now see some of that event data in a pretty graphical representation. In our first query, let's say we wanted to find out the number of ounces of coffee which was consumed. We'll select our `servingsOfCoffee` event collection, Analysis Type of `sum`, and Target Property of `numberOfOunces`. Click Run Query!

![servingsOfCoffee Query 1](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/18.png)

Keen.io just gave us the sum of all ounces of coffee served for all of the servingsOfCoffee events. Very cool! But eh.... we want a chart! So let's get the number of servings of coffee over time:

![servingsOfCoffee Query 2](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/19.png)

Your chart will probably look different than mine... I had a ton of coffee right at 2:50 PM! Note that I also used `this_30_minutes` for the timeframe; you may need to adjust this to the data you want to filter through. You can get more info on this by reading the Timeframe page on the [Data Analysis API](https://keen.io/docs/data-analysis/timeframe/).

Let's also take a look at the tabs again, now that we have data.

* **Visualization**: Our pretty chart or data visualization

![servingsOfCoffee Query 2 Visualization](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/20.png)

* **JSON**: The JSON representation of our data - note the timeframe starts and ends, and value property, so we can see this data over time.

![servingsOfCoffee Query 2 JSON](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/21.png)

* **JavaScript**: A JavaScript snippet with the parameters filled in. Please, go get the Javascript SDK and get some experience with it -- it's crazy nice! My favorite use of calling Keen.io is with their [Node.JS SDK](https://github.com/keen/keen-js).

![servingsOfCoffee Query 2 JavaScript](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/22.png)

* **Query URL**: Whoah - I said it will become a mouthful.

![servingsOfCoffee Query 2 JavaScript](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/23.png)

Let's digest the Query URL:
```
https://api.keen.io/3.0/projects/5509ba1f59949a13de470f54/queries/count?api_key=dd6bb2af8d96aae5d0b3ba99e0781ae255e4598f393d6dbe1d3875176dd6e6ddf5ba5f066ea614ab901b9f468ca3549ce64113e7be439271d1191f2497dba3324d0b029f68ee06464b3e372c384a53759c13357966a1d78e02aa75dc5f0a90f310ac31632d89a2a8f7db00be95729698&event_collection=servingsOfCoffee&timeframe=this_30_minutes&timezone=-14400&interval=minutely
```

** Base URL:**
```
https://api.keen.io/3.0/projects/5509ba1f59949a13de470f54/queries/count?api_key=dd6bb2af8d96aae5d0b3ba99e0781ae255e4598f393d6dbe1d3875176dd6e6ddf5ba5f066ea614ab901b9f468ca3549ce64113e7be439271d1191f2497dba3324d0b029f68ee06464b3e372c384a53759c13357966a1d78e02aa75dc5f0a90f310ac31632d89a2a8f7db00be95729698&event_collection=servingsOfCoffee
```

The API Key is the Read Key, and is rather long as I noted previously. But this is basically the same base URL which we used before, just with a longer API Key.

** Parameters:**
```
&timeframe=this_30_minutes
&timezone=-14400
&interval=minutely
```

These are the same parameters we picked out from the Workbench, and the same as noted in the JavaScript SDK example.

We can call this URL again with cURL, but we need to flip the URL around a bit to pass the parameters correctly:

```
curl --data "api_key=dd6bb2af8d96aae5d0b3ba99e0781ae255e4598f393d6dbe1d3875176dd6e6ddf5ba5f066ea614ab901b9f468ca3549ce64113e7be439271d1191f2497dba3324d0b029f68ee06464b3e372c384a53759c13357966a1d78e02aa75dc5f0a90f310ac31632d89a2a8f7db00be95729698&event_collection=servingsOfCoffee&timeframe=this_30_minutes&timezone=-14400&interval=minutely" https://api.keen.io/3.0/projects/5509ba1f59949a13de470f54/queries/count
```

Note, that I just took the query parameters, and passed them in with the '--data' param, and then sent in the Base URL right after. Pasting this in Terminal and hitting enter should render you the same JSON response.


## Conclusion

I really do hope this is enough to get you started using Keen.io, it's really a fantastic service and very easy to use! The analytics portion is well-developed, and you can use JSON responses however you see fit in your own custom implementation.

![Conclusion](https://raw.githubusercontent.com/markoshust/keen-getting-started/master/images/24.png)

Follow me on twitter [@markshust](https://twitter.com/markshust) to find out more about event data and other info on Keen.io, nodejs, JavaScript, and full stack development.

