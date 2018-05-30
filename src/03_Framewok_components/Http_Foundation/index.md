# The Http Foundation Component


The aim of this component is to provide som Request / Response objects you can use.

This is typically a component you'd use in controllers, that's why it's injected in controllers by default.

We thought there was a real place for a request object because it's a way to objectify, standardize and protect the access to http request parameters instead of accessing `$_GET` and `$_POST` global variables.

Due to the way WordPress works, we haven't felt the need to use the Response object yet.

## How the Request object works

The WonderWp component is a syntaxic sugar wrapper based on the symfony http foundation component. So whenever we want to figure out something about the Request inner workings, we have a look at their [dedicated documentation](http://symfony.com/doc/current/components/http_foundation.html#accessing-request-data).

## How to retrieve GET or POST parameters

The request is injected into reserved controllers by default.

Within a controller action, you can therefore do :

```
public function myControllerAction(){
	$postVariables = $this->request->request->all();
	$testGet = $this->request->query->get('test');
}
```