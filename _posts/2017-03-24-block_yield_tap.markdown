---
layout: post
title:  "Block, yield, tap "
date:   2017-03-24 19:14:47 +0000
---

## There's a sports metaphor in there somewhere

One of the most exciting things about programming (to me) is that the techniques and methods that we use as programmers are built using the programming languages and techniques that we ourselves use to write programs. Any advanced methods we use to compose a program can be accomplished using more founational methods not just because there are workarounds for every situation, but because those advanced methods were built in code using those very foundational methods. Nowhere has this become more apparent to me than in looking at the #tap method. 

I was first introduced to #tap as a a way to prevent sandwich code such as: 

```
class Pasta
  def self.create 
	  pasta = Pasta.new 
		@@all << pasta
		pasta
  end
end
```

This is sandwich code because I create a variable, assign a newly instantiated object to that variable, act on the instance, and then use the variable to return the instance. #Tap allows us to act further on the outcome of a method and to return as though the method we called #tap on was the last method called. The code above could be rewritten: 
```
class Pasta
  def self.create 
	  Pasta.new.tap {|pasta| @@all << pasta}
  end
end
```

In poking into #tap, I came across a [blog post](http://seejohncode.com/2012/01/02/ruby-tap-that/) that goes over what tap is doing behind the scenes

```
class Object
  def tap
    yield self
    self
  end
end
```

So, #tap is a method that yields the instance it's called on into a block, then returns the instance it was called on (including any changes that were made to the instance as the block executed). The person who conceived of the #tap method perhaps wrote it because they noticed that this sandwich code was a common feature of Ruby programs, and saw a way to make our code just that little bit cleaner by cutting out sandwich code. 

The code behind #tap executes the exact sandwich code that we eliminate from our code by using it.

And the beauty of programming is that we don't have to rely on the programmers who control Ruby to gain these sorts of efficiencies. We can recognize patterns that are unique to our codebases and write methods to make them more efficient and effective.
