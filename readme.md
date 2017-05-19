# Vue Flash
<p>Heavily based upon the work by Jeffrey Way at Laracasts. 
If you are interested in learning more about the lesson this package is based upon you 
may view it in his <strong><a href="https://laracasts.com/series/lets-build-a-forum-with-laravel/episodes/29">Let's 
Build A Forum With Laravel</a></strong> course. I highly recommend anyone interested in Vue or Laravel <strong><a href="https://laracasts.com/join">subscribe to Laracasts</a></strong>, it's the best $9 a month you can spend.</p>

## Setup

```javascript
    /**
     * Vue is a modern JavaScript library for building interactive web interfaces
     * using reactive data binding and reusable components. Vue's API is clean
     * and simple, leaving you to focus on building your next great project.
     */
    window.Vue = require('vue/dist/vue.js');
    
    /**
     * Our Vuejs event handler which we will be using for flash messaging
     * @type {Vue}
     */
    window.events = new Vue();
    
    /**
     * Our Flash function which will be used to add new flash events to our event handler
     * 
     * @param  String message Our alert message
     * @param  String type    The type of alert we want to show
     */
    window.flash = function(message, type) {
        window.events.$emit('flash', message, type);
    };
    
    Vue.component('flash', require('vue-flash'));

'));
    
    var app = new Vue({
        el: '#app',
    }).$mount();
```

## Usage

```javascript
flash('Hello World', 'success');
```
