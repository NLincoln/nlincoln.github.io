---
layout: post
title:  "Thoughts on React from an Ember developer"
date:   2017-10-07 13:30:27 -0500
categories:
---

I started working with ember a little over a year ago (May 2016), and it was a ton of fun to work with. I picked it up fast, and quickly rose to being my team's ember expert. I was perfectly aware of react even before learning ember, but I was off-put by JSX. The thought of having random HTML blocks all throughout my JS felt foreign and I was happy with ember's handlebars templates. 

So what changed? I learned 3 things:

## I learned JS
I learned JavaScript and ember concurrently. At the beginning of my learning, I had a hard time differentiating between what things were "just" JS and what things were provided by ember (this is a common complaint of frameworks and why I believe new developers should avoid learning with a framework).

JavaScript _completely_ transformed how I code. Previously I had only programmed in C++, where the idea of lambda functions was a new(ish) concept. However, I quickly understood how powerfuly they could be and began using them in my code (sometimes unnecessarily). Coming to JS, where defining a function was quick and painless, blew my mind at the time. It also broke me out of the C++ "funk" I was in at the time and allowed me to learn different languages (Please note I absolutely love C++ still. It was my first language and I can't wait for C++20). 



## I learned functional programming



## I learned immutable state

Or rather, I learned the _power_ of immutable state. I took one look at the [redux devtools extension](https://github.com/gaearon/redux-devtools) and fell in love with immuatable state changes. 



## Ember vs React

These are just a few things I've noticed about common patterns in react and ember.

### Props and State
In React, these two are separated apart. In ember, both of these concepts exist right on the component instance, so when you access a variable you have no idea if you're accessing a piece of state of your own component or if it's a passed in property. 

```js
// my-awesome-dropdown.js
export default Component.extend({
    isOpen: false,
    init() {
        const isOpen = this.get('isOpen');
    }
})
```

In the above, is the value of `isOpen` `true` or `false`? It's impossible to know the answer, because in ember I can just override the value of that variable with my own value:


```handlebars
{% raw %}
{{my-awesome-dropdown isOpen=true}}
{% endraw %}
```

Now the value of `isOpen` will be `true`. If you're wondering if this creates bugs, believe me, it does.


### Two-way bindings

Property bindings in ember are two-way by default. 
Building off of my previous example above, what would happen if I wrote something like this:

```js
// my-awesome-dropdown.js
export default Component.extend({
    isOpen: false,
    didInsertElement() {
        this.set('isOpen', false);
    }
});
```

Assuming I passed in `isOpen=true` to my component invocation like earlier, the value of `isOpen` 