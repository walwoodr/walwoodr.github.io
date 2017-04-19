---
layout: post
title:  "Syntactic honey and catching flies"
date:   2017-04-19 11:30:47 -0400
---

Syntactic sugar is a wonderful thing. It allows us to do many programming tasks so straightforwardly that at a certain point we forget that what we're writing is not "pure" syntax, but rather, syntax wrapped in a layer of abstraction. 

This abstraction is beautiful. It allows us to more easily read and interpreted code using a more similar linguistic context to the one we use in our day-to-day lives. 

The abstraction of syntactic sugar allows us to write functions like `number.==(5)` as `number == 5` and `5.+(3)` as `5 + 3`, both of which are abstractions that greatly increase the reader's clarity as to the intent of the function. 

This abstraction is so powerful that most of us probably don't know that when we write

```
def hats
  puts 'moon'
end
```

We are actually using a method `define_method` to define our new method. `define_method` takes a a symbol as an argument, defines the new method as that symbol, and creates the method body using the code passed to `define_method` as a block. 

```
define_method(:hats) do
  puts 'moon'
end
```

*WOAH*. This is some cool stuff. 

**The dark side of syntactic sugar**

   *Everyone knows you catch more flies with honey than with vinegar. 
   But who wants to catch flies? *
	 
Sometimes, though, syntactic sugar becomes sticky. Especially for a beginner programmer, it can be unclear when syntactic sugar is being used. Often, these are the times when you'll get stuck. 

Without a base understanding of how the syntactic honey is making a function work, it can feel like esoteric magic. The logic of the connections between the elements of the method...*is this a method?*...can get lost. You begin to feel like you need to memorize the structure of the arguments and how they're punctuated rather than building an understanding of what you're actually doing to the underlying code. Frameworks are rife with syntactic honey. Who can tell me how these methods work:

* `has_many :hats`
* `validates :moon, presence: :true`

If you're doing well, you know that `has_many` functions by adding a variety of methods to the receiving object, but is `has_many` an instance method or a class method? 

Looking at the ActiveRecord sourcecode, you'll learn that has_many is a class method (located in `Associations::ActiveRecord::ClassMethods`), and it's definied as such: 

```
 def has_many(name, scope = nil, options = {}, &extension)
       reflection = Builder::HasMany.build(self, name, scope, options, &extension)
       Reflection.add_reflection self, name, reflection
 end
```

You can read a very thorough explanation of this code in the [source code](https://github.com/rails/rails/blob/master/activerecord/lib/active_record/associations.rb#L1184)(linked 4.19.2017), or in [this blog post by Chris Callahan](http://callahan.io/blog/2014/10/08/behind-the-scenes-of-the-has-many-active-record-association/). But in either case, it's valuable to understand that the syntactic sugar that's happening here is abstracting from `self.has_many(:hats)` to `has_many :hats` 

Because has_many is called in the body of the class, the method is implicitly being called on a receiver of the class, and then syntactic sugar kicks in to allow us to call the method without the parentheses. 

The logic behind `validates` is much harder to dig into, but with a careful look at documentation and implementations of `validates` it should become clear that the syntactic sugar is abstracting from:

`self.validates(:hats, {:presence => true, :length => 6..100, :format => { :with => /\A[a-z|\s]+\z/ } ` 

To `validates :hats, presence: true, length: 6..100, format: { with: /\A[a-z|\s]+\z/ } `

The method, as we know, runs during interactions with the database (or `.valid?`), and checks that the `:hats` attribute matches all of the criteria included in the second argument, the hash.

**Returning to the light**

The key to turning syntactic honey into syntactic sugar is: 

1. Ruby contains no esoteric magic, only objects and methods. Most often syntactic honey is seen in methods.
2. What is the receiver of the method? (Is the receiver implicit?)
3. What are the arguments being passed to the method? What are their functions in the method?
4. How is the "pure" syntax being abstracted? 

If you understand how your code is being powdered with sugar, even if you don't fully understand the code that's executing, you'll find it easier to get unstuck. 
