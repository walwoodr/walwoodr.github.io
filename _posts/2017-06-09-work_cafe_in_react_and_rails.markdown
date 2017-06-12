---
layout: post
title:  "cowork café in React and Rails"
date:   2017-06-09 18:58:32 -0400
---


CoWork Café is a web application built with a React-based front end and a Rails-API-based backend.  The application allows you to share information with the community about the aspects of a coffee shop that might influence whether someone would want to work out of that coffee shop. 

To build this application, I first had to decide whether I wanted to start with a clean slate and ask users to arduously input all of the information about their favorite cafe, and myself decide on a variety of parameters with which to display those coffee shops based on unreliable user generated information, or if I wanted to utilize an API to gather basic information about cafes and display that information to users to edit. 

From the user experience side, as well as a data management perspective, it was a pretty straightforward decision -- an API it would be! 

## Hooking up with an API

### Decisionmaking

Then it was a question of which API I would use. My initial impulse was to use the Yelp API, since I'd be building a customer experience, review-esque application, and Yelp is the center of customer experience data about all sorts of public venues. However, I'd also already been exposed to the FourSquare API, so I was tempted to use that API as well, since I already knew a little bit about it and had a developer account. Looking into both of them, they seemed relatively similar, so I decided to use the Yelp API in large part because of its relevant business domain. 

### Let's get this show on the road!

After making the decision to use the Yelp API, my first task was to figure out how to query the Yelp API for data. Fortunately or unfortunately, Yelp released a new version of their API, called Yelp Fusion, around 11 months ago. It's fortunate because that timeline seems to be relatively new and fresh and exciting, but also unfortunate because it's a highly breaking change to their prior API, which means that the majority of information available about the Yelp API refers to prior versions and the version isn't frequently noted in those resources.

Initially, I planned to manage all of the API-sourced information in browser, and only push information to my database when a user had interacted with that information to add additional information. So, I spent quite a while working in JavaScript to figure out how to send a request to the Yelp API to request a token for querying, then use that token to query the database. I had everything working in Postman, but nothing working in the application itself. 

Eventually I figured out that the problem was that becasue of the token, Fetch was sending a preflight request to  the Yelp Server before it would send the actual query, and that it wasn't straightforward to configure Fetch to interact with a preflight request on a GET request. A variety of resources I looked at were very adamant that while it could be done in JavaScript, performing that request on the backend would be much more straightforward, robust, and maintainable. 

### Let's get this show on the road (take 2: GET or bust)

So I changed my tactic. I generated a Rails application, built out my migrations and models and data validations for my Cafe model...and then spent about an hour attempting to debug why Rails kept telling me it couldn't find a "Caves" table. **Pro tip:** in Rails `pluralize(2, 'cafe')` gets you `2 caves`. Once I figured that out and added a custom inflection to my Initializers, I was able to get the Yelp API query (with the help of the API's Rails example) to retrieve the information from the Yelp API and use it to instantiate Cafes in my database. 

After some tweaking, including changing my `find_or_create_by` from "name" to "address" (how many *Starbucks* are there, again?), I was ready to share that information with the front end. 

### I'm my own API 
CoWork Café is a relatively straightforward application when it comes to how it presents information: Type in a zip code, get back the coffee shops that Yelp thinks are relevant to that zip code (your mileage may vary, but this doesn't always prioritize coffee shops in the zip code). View information about any of those coffee shops, share information about any of those coffee shops, and you're done. 

In Rails routing, this broke down to: one GET index route and one PATCH show route (okay, plus one OPTIONS route, but I didn't figure that out until later).

Using the URI parameter of zipcode, I set up the index controller to use the Cafe model to query the Yelp API, instantiate Cafes, serialized the database information to JSON and respond with that information in JSON. This also required some CORS setup to allow the localhost server on which I was running my front end to access the localhost server on which I was running my backend. 

I also (and I admit this happened much later in the process, but isn't it nice to read about it all in one section?) set up the PATCH route to accept JSON from the front end application, modify the resource being posted to, and return success. This was where I encountered another preflight request, and had to figure out how to respond to it myself. **Hint:** setting up an OPTIONS route, checking whether the domain was approved by CORS on my server, then and responding with "ok". After that was done, Fetch would send the request proper, which I used to find the appropriate cafe, and update it using my Cafe model.

I also, and hopefully this comes as no surprise, used [418 as my failure code](https://walwoodr.github.io/2017/04/07/on_the_value_of_poking_around/). In an application about cafes, how could I not? 

## Make it work

### Find and display the cafes
Building out the React frontend on this application was both a delight and a struggle. I started by using static data to build out my search screen, cafe show page, and cafe edit form. Then I implemented Redux and began displaying actual data in the pages. Tweaks were made, and things started looking good. The overall interaction flow was in place.  

### React-routing
I built routing using `react-router` but little did I know that the newest version of React Router had any number of breaking changes on the version before. I implemented `history` to manage browser history and pushing browser history, as well as `react-router-dom` to implement linking. 

### Change the cafes
When I was ready to begin implementing interactions that would change the Redux store, I decided to completely scrap the pattern I had been planning to use to update cafes. I had build out a form to edit all information about the cafes, but it just...didn't look good. And it also didn't support the kind of "share what you know (not what you don't)" ethos I was hoping for the app to embody. So, rather than asking users to edit all fields about a cafe, I decided instead to allow users to edit cafe information piece-by-piece and send that information to the API on the fly. 

So, I decided to completely rebuild the inputs in a much more css-heavy, interactive way. To do that, I knew I would have to turn the Cafe Show React component into a container component, and use stateless components for each detail. Those detail components contained three conditional JSX returns apiece:
* Information is present
* Information is not present
* Information being edited

In order to implement this, I also added an "app_state" slice to my Redux state, and an "editing" state component under that -- when the "edit" link on a detail element was clicked, the Redux state changed the "editing" state to that element. When the "close" link on the element was clicked, or the data was updated, the Redux "editing" state was cleared. 

### Post the cafe changes
After changing the cafe information in the Redux store, I had to decide where to POST the changes to the cafes. I got some good advice, read some insightful articles, and eventually decided to do something that may be very anti-pattern. 

Because of how I wanted the application to work, the information about a given cafe can only be input once (this is MVP and may eventually change) and never changed. So it was less important to me that there be a tight coupling between what was changing on the front end and what was changing on the back end. In addition, in several places, changes on the front end should not propogate to the back end until the user said "save" -- but that wasn't true of all interactions. So I elected to send POST requests to the database during a lifecycle event on the Cafe React component. When the "editing" state was changing from a non-blank state to a blank state, that triggered a POST request to the API. 

**NOTE** I am aware that there are a number of downsides to the way this is implemented, first, because only one person can edit the cafe *ever*, if more than one person is on the edit page at a given time, they will not see the edits that the other user has made, and the second edits will override the first edits. This is also a problem in how I'm showing information, and is something I intend to refactor in the future. 

## Putting the shine on
I'm not going to talk about it much, but I also did a significant amount of work on this application in styling. I used Normalize as a CSS reset and Skeleton as a responsive framework. I utilized SVG icons for display and hand-built a tab-based navigation bar that only appears when there's information in the relevant tabs. I implemented a loading icon, finessed the reactive breakpoints and ended up with something that looks good, works for the scale at which I'm planning to implement it, and is easily extensible.
