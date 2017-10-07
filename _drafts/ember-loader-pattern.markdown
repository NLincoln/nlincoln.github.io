---
layout: post
title:  The Loader Component pattern in ember
date:   2017-10-07 13:30:27 -0500
categories:
---

```js
import Component from '@ember/component';
import { task } from 'ember-concurrency';
import computed from 'ember-macro-helpers/computed';

export default Component.extend({
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
{{yield data data.isRunning}}
{% endraw %}
```
