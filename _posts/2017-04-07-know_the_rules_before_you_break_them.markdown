---
layout: post
title:  "Know the rules before you break them"
date:   2017-04-07 18:45:16 +0000
---

I built a closet organizer. 
No, not like this: 

![Closet organizer made of rope and pipes](http://i.imgur.com/ZAAh8AY.jpg)

Like this: 

![File tree for Capsulize](http://i.imgur.com/d5DsiTM.png)

And I might have gotten a little bit carried away. 

The project requirements said I should build an MVC application with multiple models, at least one `has_many` relationship and user accounts. I sure did that. 

I also did this: 

![map of the Capsulize domain](http://i.imgur.com/1OO8CC2.jpg)

And part of the reason I did it is that building RESTful routes is just...pretty straightforward. Once you know what it's about, it's relatively easy to build those routes and improvise on top of them. The convention of restful routes allowed me to expand the domain past the requirements of the project without breaking my back on it. The interrelations of the models made it relatively straightforward where those models should intersect in the views.

I started by building the database migrations, models and testing the models and associations. There were a few hangups in the process, but after a few modifications to the migrations and models, we were rolling. 

After that, I sketched out the interactions I wanted the user to have with the user accounts in the controller, and built out the views. I did the same with clothing items, clothing categories, and pushed a fully functional closet organizer. 

Everything went so smoothly as I built the basic architecture of the application that I wanted to do more. Sure there were a few times I stared at the SQL statements as though they could tell me the meaning of life, and probably more than once said aloud "But that clothing_item should be valid." But the bugs faded into the background in relation to building my first from scratch web application. So I dove into building outfits into the domain. Ignoring my advice (above) to hold off on the more ambitious domain.

There were a few times I broke convention. Probably some pretty conventional ones. Instead of `'/users/new'` I used `'/signup'`; instead of `'/users/:username'`, I used `'/:username'`. Because, as with every rule, sometimes you have to break with convention.

![the code is more what you'd call guidelines than actual rules](http://i.imgur.com/VqZIuLt.gif)
