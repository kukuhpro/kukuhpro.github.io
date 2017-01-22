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
		// render view `index.ejs` file on folder `app/views`
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

### Database & Model :
*Database and Model Angel Starter is still under development, not yet 100% completed.*

Angel starter database driver is built with [CaminteJS](http://www.camintejs.com) as core package. So it's support many variants of database like mongodb, mysql, sqlite, etc. But for model on Angel is really difference between CaminteJS and Our Model. 

You can change config database on file `config/database.js`
{% highlight javascript %}
"use strict";

var path = require('path');
var root = path.resolve();
var env = require( '../helpers/env' )();

module.exports.mysql = {
    driver     : env.db.driver,
    host       : env.db.host,
    port       : env.db.port,
    username   : env.db.user,
    password   : env.db.password,
    database   : env.db.database,
    autoReconnect : true
};

module.exports.mongodb = {
    driver: env.db.driver,
    host: env.db.host,
    port: env.db.port,
    username: env.db.user,
    password: env.db.password,
    database: env.db.database,
    pool       : true
};

module.exports.sqlite = {
    driver     : "sqlite3",
    database   : root + "/storage/mySite.db"
};

{% endhighlight %}

#### Schema 
Schema in Angel starter is same as [schema in CaminteJS](http://www.camintejs.com/en/guide#define-model), you can add your CaminteJS schema in folder `app/schemas`
{% highlight javascript %}
	// Example of schema 
	module.exports = function(schema) {
	    var User = schema.define('users', {
	        username: { type: schema.String },
	        password: { type: schema.String },
	        passcode: { type: schema.String },
	        firstname: { type: schema.String },
	        lastname: { type: schema.String },
	        email: { type: schema.String },
	        mobile: { type: schema.String },
	        created_at: { type: schema.Date },
	        updated_at: { type: schema.Date },
	        deleted_at: { type: schema.Date }
	    });

	    return User;
	};
{% endhighlight %}

#### Model
Model in Angel Starter is really difference with CaminteJS model, but its pretty similar with [laravel](https://github.com/laravel/laravel) framework. 

In Angel Starter Model there are two relations between model class :

	* BelongsTo
	* HasMany

*Right know only two that already complete testing and ready to use, other relation is still under testing and development.*

But first, to define model class on Angel starter is really easy you can add, edit or delete file model inside folder `app/models`.
{% highlight javascript %} 
"use strict";

var path = require('path');
var root = path.resolve();

var Eloquent = require(root + '/helpers/cores/Eloquent');

class User extends Eloquent {
	constructor(SubdomainName) {
	   	super('User', SubdomainName); 
	}
}

module.exports = User;
{% endhighlight %}

First paramater when calling parent constructor on class model is 'User', 'User' is define as name of your Schema in folder `app/schemas`, and the second paramater is subdomainname, this subdomainname is passing paramater, its use when you are using multiple subdomain as database connection. We will discuss this in single tenant application and multi tenant database that angel starter support. :)

{% highlight javascript %}
	// Example of using model Angel Starter
	const user = new User();
	user.where({where: {name: 'Angel'}}).get((err, data) => {
		if (err) {
			console.log('ERROR' + err);
			return this.res.json({});
		} else {
			return this.res.json(data.first());
		}	
	});
{% endhighlight %}

#### Relation 
Using three relations in Angel Model class is pretty easy you just add your relation within your model class, for example
{% highlight javascript %}
'use strict';

var path = require('path');
var root = path.resolve();

var Eloquent = require(root + '/helpers/cores/Eloquent');

class Product extends Eloquent {
	constructor(SubdomainName) {
		super('Product', SubdomainName);
	}

	// add your relation 'belongsTo' model class in here
	category() {
		return this.belongsTo('app/models/Category', 'category_id');
	}

	// add your relation 'hasMany' model class in here
	sales() {
		return this.hasMany('app/models/Sales', 'sales_id', 'id', 'product');
	}
}

module.exports = Product;
{% endhighlight %}

After adding in your class model you need to calling you relation class to added within your reault of query. For example :
{% highlight javascript %}
	var product = new Product();
	// you need to add in within function 'with'
	product.with('category', 'sales').where().get((err, data) => {
		if (data == null) {
			console.log('ERROR' + err);
			return this.res.json({});
		} else {
			return this.res.json(data.all());
		}	
	});
{% endhighlight %}















