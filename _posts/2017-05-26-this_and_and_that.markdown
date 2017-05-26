---
layout: post
title:  this and => and that 
date:   2017-05-26 16:53:27 +0000
---

### Or: how you can fall in love with the best of bad options

**Realtalk:** I prefer the standard function syntax to arrow function syntax. It's more verbose, certainly, but it's also more human readable. 

```
var hat = function(lollypop){
  console.log(lollypop)
}
```

has a bit more poetry to it than:

```
var hat = (lollypop) => console.log(lollypop)
```

This is coming from someone who appreciates E.E. Cummings, so don't think I'm against strange punctuation and breaking lines apart in questionable places. I'm not, I just think there's something to be said for the additional clarity that the `function()` syntax offers. 

That said, as in poetry, the syntax mirrors the action here, and the way that the arrow function's syntax is more ambiguous echoes one of the main benefits that the arrow syntax offers you as a programmer: it's anonymity. A function declared using arrow syntax doesn't accept a *this* value. That means #1 -- if you're declaring a function within a context, you don't need to call/apply/bind an arrow function to that context as long as the function was declared in context (and in fact, you won't be able to if you want to). 

This allows us to bypass declaring a new variable in our JS objects to represent "this". 

### This won't work

Obviously this isn't a great idea, because it won't work. 

```
function Hat(){
	this.age = 4;

	setTimeout(function howOld(){console.log(this.age)}, 2000)
}

var moose = new Hat(); 
// undefined
// undefined
```

### Declaring variable to represent this

**More realtalk** Let's be honest with ourselves. `that` is useful, but it's also the linguistic equivalent to "literally" meaning both "figuratively" and "literally". It requires a lot of tracking to make sure that we're doing what we intended to do. To other people, it may as well have not been said. Avoiding this convention is a good idea where possible. 

```
function Hat(){
	this.age = 4;
	var that = this;

	setTimeout(function howOld(){console.log(that.age)}, 2000)
}

var moose = new Hat(); 
// undefined
// 4
```

### Using an arrow function

Sometimes even if you dislike a tool, it can be useful. 

```
function Hat(){
	this.age = 4;

	setTimeout(() => {console.log(this.age)}, 2000)
}

var moose = new Hat(); 
// undefined
// 4
```
