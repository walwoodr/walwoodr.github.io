---
layout: post
title:  Don't accept the magic
date:   2017-05-31 18:46:36 +0000
---

I practice a discipline when I'm reading code called "Don't accept the magic". *Don't accept the magic* means that even if you think you understand what the output of a line of code should be, if you don't understand how the code makes the output, you can't accept the magic.  

As a result, yesterday I spent about a half hour disecting what the Redux utility `combineReducers` does. Watch [this video by Dan Abramov](https://egghead.io/lessons/javascript-redux-implementing-combinereducers-from-scratch) to understand what I'm talking about. The video goes fast, and explains this lovely bit of code: 

```
function combineReducers(reducers){
  return (state = {}, action) => {
    return Object.keys(reducers).reduce(
      (nextState, key)=>{
        nextState[key] = reducers[key](state[key], action);
        return nextState
      }, {}
    )
  }
}
```

It's a complex bit of work that turns multiple Redux reducers that work on slices of state into a single Redux reducer that works on the entire Redux state. There's very little in it that is individually complex, but as a whole, it is complex. Despite the time it took me to analyze it, I'm glad I did. Every time I refuse to accept the magic, I come away with strengthened knowledge of development patterns. 
