# Services

The idea behind services is to try to provide a way to cleanly separate all the things you could find in a functions.php file and tidy them in several classes.

This approach tries to follow several object-oriented programming principles like:

- Single Responsibility Principle
- Open Close Principle
- Liskov Substitution Principle

With that in mind, services are little classes here to help you address a specific programming aspect of your plugin, like hooks, routing, assets management, api calls, tasks management, shortcodes, and so on.

This way, if you need to work with hooks in your plugin, you'll know there will be in the HookService, if you need to work with shortcodes, they will be in the ShortcodeService, if you need to manage assets, they will be in the AssetService, and so on.

Some service names are pre-defined, but you can also create your own ones.

Here's the list of [reserved service names](https://github.com/wonderwp/Service/blob/develop/src/ServiceInterface.php) you can use.

## Creating a service

If it's a reserved name service, create a class that will extend the relevant service family abstract class. For example:

```
class MyPluginHookService extends AbstractHookService
{

}
```

If you want to create your own, you can extend the `AbstractService` class if you like. Doing so allows you to inject the manager reference if you want to. But it's not mandatory.

```
class MyPluginCustomService extends AbstractService
{

}
```

## Registering a service

This takes place in the manager, referenced as `$this` in the following snippet.

```
$this->addService(AbstractService::$HOOKSERVICENAME,$container->factory(function(){
	//My Plugin Hook service
   return new MyPluginHookService($this); //Pass $this in the constructor if you want to access the manager inside your service.
}));
```

The `addService` method takes two arguments, a name, and a closure that returns the object instance.

## Calling a service

Your service is registered inside your manager. To get the manager's service, you first need to get your manager back (from dependency injection for instance).
Then you can call the `getService` method like this `$myPluginService = $manager->getService($serviceName)`;


# What if I want to ...

Once your plugin has been generated of that you've created your plugin architecture, it's time for you to take over and start building your plugin.

In WordPress, we often retrieve common tasks to perform such as adding hooks, assets, shortcodes, working with an ajax endpoint, a router...

So what if I want to...

## ...add a hook
If your plugin doesn't have a [hook service](./01_Hook_service.md) yet, create one and register it in your manager (see the doc for this in the services > Hook Service section)

Add your hook inside the HookService `register` method

## ...add a shortcode

If your plugin doesn't have a [shortcode service](./06_Shortcode_service.md) yet, create one and register it in your manager (see the doc for this in the services > Shortcode Service section)

Add your shortcodes inside the AssetService `registerShortcodes ` method

## ...add assets

If your plugin doesn't have an [asset service](./03_Assets_service.md) yet, create one and register it in your manager (see the doc for this in the services > Asset Service section)

Add your assets inside the AssetService `getAssets` method

## ...create a new route

If your plugin doesn't have a [route service](./02_Route_service.md) yet, create one and register it in your manager (see the doc for this in the services > Route Service section)

Add your assets inside the RouteService `getRoutes` method

## ...work with Ajax or an API endpoint

If your plugin doesn't have an [api service](./04_Api_service.md) yet, create one and register it in your manager (see the doc for this in the services > Api Service section).

Every public method defined in this file can then be used as an ajax endpoint.

## ...create a wp-cli command

If your plugin doesn't have a [task service](./05_Task_service.md) yet, create one and register it in your manager (see the doc for this in the services > Task Service section)



Well as you can see, the WonderWp philosophy encourages you to create a dedicated service for each capability you'll add to your plugin.
