---
layout: post
title:  Minimalist code
date:   2017-05-12 19:08:29 +0000
---


I'm a minimalist. If you read blogs or the New York Times, you might think that that means I live in a stark white room with no objects around. However in my opinion, Minimalism is better summed up using this quote: 

> If you want a golden rule that will fit everybody, this is it: Have nothing in your houses that you do not know to be useful, or believe to be beautiful.
> - William Morris 

Life is complex, and minimalism advocates for a regular review of the objects you own to ensure that you're not cluttering up your space with things you don't need (or even want). 

Hopefully you can see the parallels with code. Refactoring your code is cleaning out your closet. That specific method you only use in one place is a [bannana slicer](https://www.buzzfeed.com/juliegerstein/just-do-yourself-a-favor-and-read-these-banana-slicer-review). The set of routes you generated using `rails g scaffold` is the extra set of dishes you keep in your basement because you're not sure when you might need them. 

And like minimalism is pretty easy for a teenager or even a college student (who cares to pursue it), minimalism in coding is pretty easy for someone learning to code and whose only projects have two or three models, no users, and often, no deployment. It gets harder once you have a house (operations level infrastructure) and a couple of kids (a product manager and customers). 

## Strategies for success

The best strategies for minimalism are the same as the best strategies for quality code: 
* **Keep an eye on what you bring into your house** -- Red-Green-Refactor is a solid strategy -- but make sure it never becomes Red-Green-Deploy-Refactor. Because that's likely to turn into Red-Green-Deploy. Keep in mind that [phase 2 is the biggest lie in Corporate America](https://hbr.org/2012/05/the-biggest-lie-in-corporate-a).
* **Get rid of things when you notice that you don't need them** -- If you're working on a new feature and you come across code that needs fixing, fix it! Don't leave it there with a note to deal with it later, don't shove it under a rug. Fix it. Fix it now. 
