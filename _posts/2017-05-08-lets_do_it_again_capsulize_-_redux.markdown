---
layout: post
title:  "Let's do it again (Capsulize - redux)"
date:   2017-05-08 17:38:25 -0400
---


I built the same application twice. Don't get me wrong, it's not exactly the same. The new version has some styling, includes a few additional bells and whistles, and most importantly, is tested end-to-end. 

As I started building Capsulize version 2.0, I decided to poke around a bit in capsulize version 1.0, to make sure I was getting the same basic functionality, but improving on it. I hadn't touched the code in about 3 weeks, and I thought surely it will be materially the same. The features and functionality that I'd built will remain in place, nothing to worry about. 

I could not have been more wrong. 

Three weeks before, I'd poked at the app a little bit -- fixed a bug where you could sign up twice with the same username, added a partial or two. And when I went to poke at it I realized that I'd broken the app more than I would have though. 

I've spent some time doing manual QA on software and I can tell you that while developing a project on my own I already have to do product management, project management, design, and development. I'd prefer the QA side of things be as small as possible. 

So I set myself a challenging task with Capsulize as I was building it on Rails: 

## End-to-end Testing
I definitely did not know what I was biting off when I made the decision to build out my rails app with end-to-end testing. But I  made the decision, and along the way it proved to be the right one. 

I used RSpec as my testing suite, with a sprinkling of Capybara, Faker and FactoryGirl. 

Using Test Driven Development, I started out with **Model Specs** for the domains I planned to include in my application, and wrote around 30 tests to ensure they were functioning as expected. Having mostly used RSpec previously as a tool to interpret what code I should be writing, it took some doing to make sure that I was writing tests that could be passed by implementing the features I wanted to implement. Getting syntax right and data right were challenges. I relied primarily on seed data in my model specs. Once the tests were failing in semantic ways, I began to implement my models. 

Using Rails and Devise, getting my models working was possible in relatively short order. 

Then I dove into **Controller Specs**. I read several blog posts about how to write good controller specs, and while I struggled with how to implement controller specs that didn't dribble over into feature specs, I was able to implement a few controller specs that test page routing and rendering. I used Warden for authentication in my controller specs as well as feature specs. While writing controller specs, I relied heavily on FactoryGirl for mock data.

Again, using Rails, it was possible to get the controller specs passing in relatively short order. 

*Note:* This first iteration of Capsulize in Rails is substantially authorization-light. I relied on nested routes and the `current_user` method provided by Devise rather than implementing a thorough authorization scheme.

Finally, I dove into **Feature Specs** with a few **Helper Specs** sprinkled in for good measure. I wrote around 35 feature specs, and largely found these to be the most straightforward specs to write. I again primarily used FactoryGirl to mock my test data, and of course relied on Capybara to introspect on the DOM. For my feature specs I worked model-by-model, starting with the largest featureset (the Outfit model) and working my way into the supporting models (Categories, Clothing Items and users). 

As I wrote feature specs and implemented features, I also dipped occasionally into helper methods in order to implement repetative view logic. I also wrote tests for those helper methods. 

In the end, I have 89 tests and an application I feel excited to work on going forward. 
### Why you should write tests too
Several times, as I implemented a feature for one model, I knowingly broke a feature in a different model. Having written tests, I was able to run my test suite and pinpoint the areas that had broken and quickly fix them, rather than promising myself I would remember to fix them. I only had to remember to run the full test suite after implementing the feature so that I could fix the problem I'd caused. 

Testing is hard. This project took me twice as long as it might have if I hadn't committed to end-to-end testing at the outset. But it has given me the brainspace to work my code more vigorously, with the knowedge that I don't have to rely on my memory of how the code works in order to prevent breakage. It allows me to refactor with the confidence that I will be able to pinpoint any dependences and resolve them. It gives me the ability to focus on writing the best code I can, rather than splitting my attention between solving the problem I'm working on, and resolving the dependency I just broke. And that focus is a beautiful thing. 
## Fancy Footwork
#### Or: the brag section
If there's one piece of code that I'm most proud of in Capsulize v2.0, it's this bit of magic I worked with a partial that I wanted to use as a nested field as well as an independent form. To use the form fields code in both places, I did as follows:
1. created a partial ["form_fields" for the clothing_item class, ](https://github.com/walwoodr/capsulize-rails/blob/master/app/views/clothing_items/_form_fields.html.erb) 
2. used the form_fields with a local "c" in a[ fields_for embeded in a form_for](https://github.com/walwoodr/capsulize-rails/blob/master/app/views/outfits/_form.html.erb) in the form partial used to add and edit outfits
3. used the [form_fields partial in a form_for](https://github.com/walwoodr/capsulize-rails/blob/master/app/views/clothing_items/_form.html.erb) in a form partial used to add and edit a clothing item.  

