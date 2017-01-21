---
layout: post
title: "Angel, Express JS 4 Starter Kit with taste of a Framework"
date: 2017-01-20 00:00:24 +0700
categories: javascript nodejs framework expressjs starter
tags: javascript nodejs expressjs framework starter es6 object oriented design pattern kukuhprabowo
depth: 0.06
---
### Let's Roll !
Angel is Web Application Starter for NodeJS with ExpressJS Framework. This starter built with heart of believe that development must be an easy, enjoyable and creative. This starter is inspired a lot by [Laravel Framework](https://github.com/laravel/laravel). Angel starter attempts to take out the pain out of development using nodejs with expressjs. 

Checkout on [Angel Github](https://github.com/kukuhpro/Angel-ExpressJS4-Starter), and for how to use it ? check documentation below :

### Routing : 
Angel starter routing is on folder `app/routes`, you can create all files of routing `.js` file inside folder `app/routes`, Angel will automatically read those files and write them as routing on expressjs. Example route file `site.js` :
{% highlight javascript %}
module.exports = [
	{
		get: {url: '/', middleware: "AuthIsLogin", as: 'angelsite', folder: 'site', uses: 'home@test3'}
	},
	{
		post: {url: '/form-post', as: 'site.form.store', validation: 'LoginRequest', folder: 'new', uses: 'home@storeForm'}
	},
	{
		group: {
			options: {prefix: '/secure', middleware: [], folder: 'site'},
			methods: [
				{
					get: {url: '/gerimis', middleware: [], as: 'site.home.secure', uses: 'home@test'}
				}
			]
		}
	}
];
{% endhighlight %}

On this route first key is declare as a method on route, and object on key are available options.
{% highlight javascript %}
{
	post: {url: '/form-post', as: 'site.form.store', validation: 'LoginRequest', folder: 'new', uses: 'home@storeForm'}
}
{% endhighlight %}

you can also add group routing, in case you need to add prefix, middleware, or even folder.
{% highlight javascript %}
{
	group: {
		options: {prefix: '/secure', middleware: [], folder: 'site'},
		methods: [
			{
				get: {url: '/gerimis', middleware: [], as: 'site.home.secure', uses: 'home@test'}
			}
		]
	}
}
{% endhighlight %}

### Controllers :
Controller in Angel starter is inside folder `app\controllers`, for example let's just say you have this on route : 
{% highlight javascript %}
{
	get: {url: '/', folder: 'site', uses: 'home@index'}
}
{% endhighlight %}
on this route it will say like this `find controller method function index in class controller home inside folder site`, so basically we will need class controller inside folder site. This how class controller home `home.js` looks like inside folder site.

{% highlight javascript %}
'use strict';
var root         = require('path').resolve();
var controller = require(root + '/helpers/cores/Angel/Controller');

class home extends controller {
	constructor() {
		super();
	}

	index() {
		return this.res.render('index', {});
	}
}

module.exports = home;
{% endhighlight %}

Here some options in your controller that you can do 
{% highlight javascript %}
	// redirect to url `/form` 
	return this.res.redirect('/form');

	// redirect to specific routing with an alias `angelsite` 
	return this.res.redirect(this.url.route('angelsite'));

	// redirect to your previous url
	return this.back();

	// access `.env.json` file
	var AppName = this.env.appName

	// sending email
	this.mailsend(data, function (err, info) {
		// do something on callback
	}.bind(this));
{% endhighlight %}






