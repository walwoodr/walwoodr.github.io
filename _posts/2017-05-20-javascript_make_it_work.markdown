---
layout: post
title:  JavaScript 💅: Make it work
date:   2017-05-20 15:26:36 +0000
---

"Make it work" is one of my favorite phrases. It can mean anything from *It's broken, make it work* to *It's good, but you could make it **work***. When JavaScript comes onto the scene, we say make it work, and we mean the second. JavaScript can absolutely take a technology need from 0 to done, but my experience (and the experience of most modern webmedia consumers) is that JavaScript makes good better. 

That said, my experience of making my Rails app Capsulize **work** using JavaScript, also involved a significant helping of making it *work*. So, let's talk a little bit about the ways I made it work: 

## Mistakes Were Made

As I built a JavaScript frontend to layer on top of my Rails backend, I had any number of challenges. Let's talk about those, and how I solved them. 

### Mistake 1: Parentheses!!!

Syntax is a funny thing. I spent 10 minutes watching my DOM be updated with the content I wanted and then watching that content almost immediately disappear. The content was there. If I added it in debugger, it stayed. Something happened in the milisecond immediately after the content was added to take it away. If you've been programming for any length of time, you're probably screaming "Prevent Default!" In your head, and so was I. I'd prevented the default. Or so I thought. Actually, though, I'd only referenced the function. `e.preventDefault;` and `e.preventDefault();`

I'm not proud to say it, but I worked around the problem by removing the href from the link. Success(ish). After a night's sleep, I came back to it with fresh eyes, and discovered the issue. 

### Mistake 2: Riding on someone else's Handlebars

This one tripped me up more than I'd care to say. I used handlebars templates in a number of views in order to render resources. So clever, I thought, so efficient! Not so fast. The asset pipeline, as we know, loads all of the javascript in the project and caches it for the website. If my JavaScript refers to a handlebars template on pageload that's not on the page that's loaded, it will **never be available** to the JavaScript unless you refresh in a location where that template is available. Big problem. 

Because of the relatively small size of my app, I decided to add those handlebars templates as partials to my layout file. I could, instead, have loaded my JavaScript differentially based on when it was needed. In future, as the app grows, I may do that. Right now, I didn't. 

### Mistake 3: Make me a promise

My queries were chugging right along, loading up data, tossing it into the console to make sure it was there and then...gone. Not available to the functions in which I was referring to it. After some poking (and lots of `console.log()` I realized that the problem was that the request was completing after the second function had fired. Which made sense, but I wasn't sure how to fix it. I made it work by chaining functions in my callback, but generating the HTML and adding it to the DOM didn't seem to belong in my query function. 

I tossed this one to a developer friend of mine who said "You need to use a Promise". I'd heard of this elusive promise. I even had a sense that I'd used one before. Was maybe even using one now... 

Nope. A request callback is not a promise! Phew! Good to know (I'll tell some actor friends). I returned my query on a function, added a `done()`, and just like that, my concerns were seperated and my app was working. 

## Conclusion
Tim Gunn would be proud: I worked through my problems, I hit my deadline, I made it work and if there are a few places that I could have trimmed threads or added a zipper, that's always going to be true. 
