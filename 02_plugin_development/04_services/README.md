# 04\_Services

## Services

The idea behind services is to try to provide a way to cleanly separate all the things you could find in a functions.php file and tidy them in several classes.

This approach tries to follow several object-oriented programming principles like:

* Single Responsibility Principle
* Open Close Principle
* Liskov Substitution Principle

With that in mind, services are little classes here to help you address a specific programming aspect of your plugin, like hooks, routing, assets management, api calls, tasks management, shortcodes, and so on.

This way, if you need to work with hooks in your plugin, you'll know there will be in the HookService, if you need to work with shortcodes, they will be in the ShortcodeService, if you need to manage assets, they will be in the AssetService, and so on.

Some service names are pre-defined, but you can also create your own ones.

Here's the list of [reserved service names](https://github.com/wonderwp/Service/blob/develop/src/ServiceInterface.php) you can use.

### Creating a service

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

### Registering a service

This takes place in the manager, referenced as `$this` in the following snippet.

```
$this->addService(ServiceInterface::HOOK_SERVICE_NAME,$container->factory(function(){
	//My Plugin Hook service
   return new MyPluginHookService($this); //Pass $this in the constructor if you want to access the manager inside your service.
}));
```

The `addService` method takes two arguments, a name, and a closure that returns the object instance.

### Calling a service

Your service is registered inside your manager. To get the manager's service, you first need to get your manager back (from dependency injection for instance). Then you can call the `getService` method like this `$myPluginService = $manager->getService($serviceName)`;
