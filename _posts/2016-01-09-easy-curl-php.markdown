---
layout: post
title: "Easy Curl PHP"
date: 2016-01-10 00:00:24 +0700
categories: php curl 
tags: curl php thirdparty package composer kukuhprabowo
depth: 0.06
---
It begins when working some projects to connect many thirdparty service via http connection, I create this curl abstract class to make this development of this project more easier. Well, i'm pretty lazy to create curl configuration one by one everytime to connect with thirdparty. So i ended up created Abstract class for handle the curl configuration. 

#### Available Configuration
{% highlight ruby %}
protected $header             = []; //This what header configuration for http service

protected $method             = 'POST'; // This is for what method that used

protected $json               = true; // if this value is true and this method is post then all data will sent it in json data type

protected $connection_timeout = 10; // how long connection timeout

protected $https              = false; // if this value is true then http will change to https

protected $http_query         = false; // if this value is true then all data will sent via http build query

protected $endPoint; // this is a url endpoint

protected $apps; // this application names
{% endhighlight %}


#### How To Use it
Just put this on your composer.json files
{% highlight ruby %}
{
  "require" : {
    "kukuhprabowo/curl-abstract" : "dev-master"
  }
}
{% endhighlight %}
###### *Don't Forget to composer update/composer install
and this is a tutorial how to used this package with an examples. 

#### Example
In this example i'm gonna use this abstract class for Google Cloud Messaging Http service. Let's say we have a project that required send notifications using GCM to mobile application android. 

##### 1. Create Class
{% highlight ruby %}
<?php 
// require autoload from vendor composer json
required_once("vendor/autoload.php")

// Calling AbstractCurl
use Kukuhprabowo\AbstractCurl;

// Create class for thirdparty Service, in this case Google Cloud Messaging
class GCM extends AbstractCurl {

}
{% endhighlight %}

##### 2. Setting a Global Configuration for Thirdparty Http Service
{% highlight ruby %}
// create construct function inside class
public function __construct()
{
    $this->apps       = 'Google Cloud Messaging Application';  // Set name of your application
    $curl_set_options = [
        CURLOPT_CONNECTTIMEOUT => 30,
        CURLOPT_TIMEOUT        => 20,
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_HEADER         => true,
        CURLOPT_FAILONERROR    => false,
    ]; // set global configuration for curl options
    $header = [
        'Authorization: key YOUR_API_KEY',
        'Content-Type: application/json',
    ]; // set global header configuration, in this case is authorization and content type json.
    $this->endPoint = "https://gcm-http.googleapis.com/gcm"; // set your url endpoint for this application
    parent::__construct($curl_set_options, $header); // the last thing we call construct parent and added curl options and header. 
}
{% endhighlight %}

##### 3. Create a Function Inside Class
{% highlight ruby %} 
public function sendNotifications($params = [])
{
    $this->method     = "POST"; // Set this method for this request
    $this->json       = true; // send data via json type
    $this->http_query = false; // set false http query
    $service          = '/send'; // added endpoint to put into base url

    $this->setup($service, $params); // Calling this function to tell abstract class prepare the configuration

    $data_response = $this-_send(); // call send function and it will return ObjectClass with header and body on it. 

    // well the rest is up to you, what do you want to handle with this data response, for me let's just return data response
    
    return $data_response;
}
{% endhighlight %}

##### 4. Let's Call This Thirdparty Service
{% highlight ruby %}
$gcm = new GCM(); // calling a new class
$params = [
    "data" => [
        "score" => "5x1",
        "time" => "15:10",
      ],
      "to" => "bk3RNwTe3H0:CI2k_HHwgIpoDKCIZvvDMExUdFQ3P1...",
    ]
];

$data = $gcm->sendNotifications($params); // handle all response into data variable
var_dump($data); // dump this data
{% endhighlight %}

Well, it just four steps to use this package, how easy is that. 

But I know this is package also need some improvements so if you want make this package more awesome then just visit [this github page](https://github.com/kukuhpro/curl-abstract) and create a pull request, i'll makes sure your pull request is gonna make this package more awesome.

### Laravel Way
{% highlight ruby %}
<?php namespace MyApp\MyService;  

use Kukuhprabowo\AbstractCurl;

class GCM exteds AbstractCurl {

}
{% endhighlight %}


