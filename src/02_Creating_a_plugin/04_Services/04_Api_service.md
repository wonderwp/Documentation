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

Based on https://developer.wordpress.org/rest-api/extending-the-rest-api/adding-custom-endpoints/

Supposed we want this endpoint `https://domain.test/wp-json/questions/v1/questions-by-user?page=1` the annotation would be :
- **namespace** : corresponding `questions`
- **version** : corresponding to `v1`, we can maintain different api version by incrementing the version and keeping the same endpoint url (default to `v1`)
- **url** : corresponding to `questions-by-user`, can include dynamic parameters according to wordpress documentation (example `/author/(?P<id>\d+)`)
- **args** : corresponding to third parameter of `register_rest_route` function : [doc](https://developer.wordpress.org/rest-api/extending-the-rest-api/adding-custom-endpoints/#arguments)

```php

 class QuestionApiService extends AbstractApiService
{
    /**
     * @WPApiEndpoint(
     *     namespace = "questions",
     *     version = "v1",
     *     url = "/questions-by-user",
     *     args = {
    *       "methods": "GET",
    *       "args": {
    *           "page": {
    *               "default": 1,
    *               "sanitize_callback": "convertToInt"
    *           },
    *           "limit": {
    *               "default": 1,
    *               "sanitize_callback": "convertToInt"
    *           }
    *       },
    *       "permission_callback": "canFetchQuestions"
    *     }
     * )
     */
    public function questionsByUser(WP_REST_Request $request)
    {
        $page = $request->get_param('page');

        /* Processing get questions */

        // To handle error we can return WP_Error instance
        return WP_Error('error_code', 'Error message', ['status' => 400]);

        // Or success response
        return new WP_REST_Response([
            'questions' => [],
        ]);
    }

    public function canFetchQuestions(WP_REST_Request $request)
    {
        $user = wp_get_current_user();

        /* processing verification */

        return true;
    }

    public function convertToInt($param, $request, $key)
    {
        /* Processing sanitize method */
        return (int) $param;
    }
}
```

The namespace could be define globaly.
For example to create this endpoint `https://domain.test/wp-json/questions/v1/delete-question/1`

```php
/**
 * @WPApiNamespace(
 *     namespace="questions"
 * )
 */
class QuestionApiService extends AbstractApiService
{
/**
     * @WPApiEndpoint(
     *     url = "/delete-question/(?P<id>\d+)",
     *     args = {
     *      "methods": "DELETE",
     *      "permission_callback": "canDeleteQuestion"
     *     }
     * )
     */
    public function deleteQuestion(WP_REST_Request $request)
    {
        /* Processing deletion */
    }

    public function canDeleteQuestion()
    {}
}
```

### Important notice

Because we cannot reference `$this` from within an annotation, for any callback function specified inside a @WPApiEndpoint annotation, be it `sanitize_callback`, `validate_callback`, `permission_callback` and so on, the provided callback name (for example `myFunction`), is first looked for on the same class instance than the method carrying out the annotation. If not found there, it's then looked for on the global namespace (like a function name)
In second time, we look for a global function.


Here's another example :

```php
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
