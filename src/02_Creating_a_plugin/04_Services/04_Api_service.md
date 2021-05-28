# Api Service

Defining ajax entry points for your plugin can feel a bit messy in a sense that it works with hooks. Those hooks could be defined at multiple places (unless you create a hook service), and for each hook defined, you associate a callable that can be a function, an object method somewhere, or somewhere else.

The idea behind the Api Service is to provide a class that will act as an API controller to gather all the API logic, and that can be used to abstract and ease ajax endpoints registration.

## How to create an Api Service
Create a class that extends the `AbstractApiService` class. The `AbstractApiService` class implements the `ApiServiceInterface`.

```
class MyPluginApiService extends AbstractApiService
{
	//All the public methods defined in this service can be used as an ajax endpoint
}
```
All the public methods defined in this service can be used as an ajax endpoint! No more hook definition, you just create the API service, and then declare public methods in it. Those methods can now be accessed via JavaScript calls.

## Registering the Api Service
Add these few lines inside your plugin manager.

```
//Api
$this->addService(AbstractService::$APISERVICENAME, function(){
    return new MyPLuginApiService();
});
```

## The two types of ajax calls

You API service can be used to work with the old ajax API, or the new REST one.

The two different types are documented below.

### Basic ajax calls via the old admin-ajax endpoint (wp-admin/admin-ajax.php)

To access your ApiService methods from your JavaScript files for ajax calls, make an ajax call to the ajaxurl global variable with parameters like this :

```
$.post(
	ajaxurl, //The ajaxurl global variable
	{ 
		action: 'MyApiServiceName.myApiServiceMethod', //This will locate your api service method
		otherParamKey: otherParamVal //This is some other parameter you can provide
	}, 
	function (response) {
        //Deal with the reponse	    
	}
);
```

In this call, pass some object parameters. This object should contain an action key, whose val should be the name of your api service class, then a '.', then the method you'd like to call. 

You can also define som more parameters that will be passed to the service's method.

#### Result

In an attempt to standardize api call responses, we've created a Result object that takes two arguments, an http response code, and an array of data.

In practice, in your api methods, you could end the method with something like this:

```
$response = new Result(
	200, //Your HTTP code
	[ 
    	'settings'=>$formView,
    	'visibleFields'=>$visibleFields
	] //Your array of data
);

return $this->respond($response); //The respond method is also a standard method to end api calls
```

I tend to find this Result object practical, and to use it even in not api related actions, (like form submission services for example).

## REST API Endpoints ajax calls (via the WP REST API)

It's also possible to use your API service to make its methods available via the WP REST API as endpoints.

Here's an example on how to do so :

```
<?php

namespace WonderWp\Plugin\Cache\Service;

use WonderWp\Component\API\AbstractApiService;
use WonderWp\Component\API\Annotation\WPApiEndpoint;
use WonderWp\Component\API\Annotation\WPApiNamespace;
use WP_REST_Response;

/**
 * @WPApiNamespace(
 *     namespace="cache"
 * )
 */
class CacheApiService extends AbstractApiService
{
    /**
     * @WPApiEndpoint(
     *     url = "/me",
     *     args= {
     *      "methods": "GET"
     *     }
     * )
     */
    public function cacheMe()
    {
        $personalCache = apply_filters('wwp-cache.me', []);
        return new WP_REST_Response([
            'code' => 200,
            'data' => $personalCache
        ]);
    }
}

```

The combination of `@WPApiNamespace(namespace="cache")` and `@WPApiEndpoint( url = "/me")` make the `cacheMe()` method available behind a GET request to the url `/wp-json/cache/v1/me`
