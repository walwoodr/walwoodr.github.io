---
layout: post
title:  "On the value of poking around"
date:   2017-04-07 18:01:41 +0000
---


Are you a teapot? Because [this Google page](https://www.google.com/teapot) is. And so can your route's status response be. 

While learning about Rack, I was poking around at the methods that Rack gives to your response object, and as I scanned through the methods, one caught my eye:

![:i_m_a_teapot?](http://i.imgur.com/U78mkgt.png)
**:i_m_a_teapot?**

This little easter egg led me on a bit of a chase whereby I discovered that this refers to a status code that your server passes to the client. It's a 400 status, meaning the page is a client error. In this case, error 418 is the "I'm a teapot" error. The origin of this error is [an April Fool's Day joke](https://sitesdoneright.com/blog/2013/03/what-is-418-im-a-teapot-status-code-error), but it's also an honest to goodness response status that you can use. As you can see, Google has. Also, apparently, [Stack Overflow](https://meta.stackexchange.com/questions/185426/stack-overflow-returning-http-error-code-418-im-a-teapot). 

After my discovery, I poked at it a bit more: 

![resp.status = 418; resp.i_m_a_teapot? => true](http://i.imgur.com/jBLhj0F.png)

The web is a wild and wonderful place.
