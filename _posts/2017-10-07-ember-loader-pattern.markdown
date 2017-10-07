---
layout: post
title:  The Loader Component pattern in ember
date:   2017-10-07 13:30:27 -0500
categories:
---

This is a post about a pattern I find useful in my ember.js projects: I call them "loaders". The idea is that you have an `ember-concurrency` task that loads data from ajax, or the store, or whatever async source you want, and a computed property that calls `perform` on that task. They usually look something like this:

```js
// components/user-loader.js
import Component from '@ember/component';
import { task } from 'ember-concurrency';
import computed from 'ember-macro-helpers/computed';
import { inject } from '@ember/service';

export default Component.extend({
    store: inject(),

    loadData: task(function* (params) {
        return this.get('store').query('users', params);
    }),

    data: computed('foo', function(foo) {
        return this.get('loadData').perform({
            foo
        })
    })
});
```
```hbs
{% raw %}
{{! components/templates/user-loader.hbs}}
{{yield data data.isRunning}}
{% endraw %}
```

And you can use them like this:

```hbs
{% raw %}
{{#user-loader foo="bar" as |user isLoading|}}
    {{#if isLoading}}
        {{loading-spinner}}
    {{else}}
        {{user.value.name}}
    {{/if}}
{{/user-loader}}
{% endraw %}
```

There are several useful things about this:
- You can put some model-specific logic in the loader component, so you don't have to use it elsewhere
- It takes loading of async data out of the model hook in your route, which means rendering isn't blocked
- It's _super_ easy to handle loading states, unlike the route-level loading hook which can get complicated quickly
- Because it uses computed properties, it will automatically re-load the data every time a param changes. 
