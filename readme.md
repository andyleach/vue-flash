# Vue Flash
<p>Heavily based upon the work by Jeffrey Way at Laracasts. 
If you are interested in learning more about the lesson this package is based upon you 
may view it in his <strong><a href="https://laracasts.com/series/lets-build-a-forum-with-laravel/episodes/29">Let's 
Build A Forum With Laravel</a></strong> course. I highly recommend anyone interested in Vue or Laravel <strong><a href="https://laracasts.com/join">subscribe to Laracasts</a></strong>, it's the best $9 a month you can spend.</p>

<h3><a href="http://vueflash.leachcreative.com/">Demo</a></h3>

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
    
    var app = new Vue({
        el: '#app',
    }).$mount();
```

## Usage

```javascript
flash('Hello World', 'success');
flash('Hello World', 'error');
flash('Hello World', 'warning');
flash('Hello World', 'info');
```

### Customizing Alert Classes
<p>By default, <strong>Vue-Flash</strong> uses the alert classes from Twitter Boostrap, however it also supports the ability to integrate with the css framework of your choice using the types prop.  Using this prop you can assign alert classes to a specific alert type as an object as example of which can be found below.</p>

```javascript
{
    base:    'alert',
    success: 'alert-success',
    error:   'alert-danger',
    warning: 'alert-warning',
    info:    'alert-info'
}
```

### Supporting Icon Libraries
<p>By default, <strong>Vue-Flash</strong> does not include any icon libraries because I wish to keep dependencies to an absolute minimum so that the user of the package can decide what to include and what to not include.  If however you desire to include a icon library this feature can be turned on if you set the displayIcons(:display-icons="true") property to true.  Default values for what icons(Font Awesome) are used for a alert type have been preset to for success, error, warning and info.  If you wish to set your own you can use the same format as when setting alert classes.  See below for an example:

```javascript
{
    base:    'fa',
    success: 'fa-check-circle',
    error:   'fa-exclamation-circle',
    warning: 'fa-exclamation-circle',
    info:    'fa-info-circle'
}
```

### Props

| Name       | Type    | Default        | Restrictions                  |
|------------|---------|----------------|-------------------------------|
| timeout    | Number  | 3000           |                               |
| transition | String  | slide-fade     | fade, slide-fade, bounce      |
| types      | Object  | See above      |                               |
| icons      | Object  | See above      |                               |
## License
<p>Vue-Flash is open-sourced software licensed under the <a href="http://opensource.org/licenses/MIT">MIT license</a>.</p>
