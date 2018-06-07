# Hook Service

Potentially, in a large plugin, you could find all sorts of hooks at many different places. The idea to have a hook service is to have a central place where all hooks are stored cleanly.

This service is then responsible for registering all the plugin hooks, and then dispatch the hook execution to the required classes.

## How to create a hook service
Create a class that extends the `AbstractHookService` class. `AbstractHookService` implements the `HookServiceInterface`, therefore it requires that your hook service implements a `run()` method.

```php
class MyPluginHookService extends AbstractHookService
{
	public function run(){
		//Register your hooks from there
	}
}
```

## Registering the hook service
Now that your hook service is ready, add these few lines inside your plugin manager to let him now about your hook service.

```
$this->addService(AbstractService::$HOOKSERVICENAME,$container->factory(function(){
    //Hook service
    return new MyPluginHookService($this);
}));
```

## Adding the hooks

Within the run method, add your actions and filters. Ideally the callable used by the hooks should be called on a dedicated service instead of being just a functino that lies randomly a few lines later.

For example :

```
class MyPluginHookService extends AbstractHookService
{
	public function run(){
		//Register your hooks from there
		
		$adminUiService = $this->manager->getService('adminUi');
		
		$this->addAction('my_action_a', [$adminUiService,'handleActionA');
		$this->addAction('my_action_b', [$adminUiService,'handleActionB');
		
		$this->addFilter('example_filter', [$adminUiService,'filterSampleValue')
		
	}
}
```

Note the use of `$this->addAction` and `$this->addFilter` instead of the regular `add_action` and `add_filter` procedural calls. We've done so to try inject as few procedural WordPress references as possible within your plugin code. But that's not the only reason. Behing those two calls, your hook services delegates the action and filter registration to another object called the `HookManager`. 

## The Hook Manager

This `HookManager` is a behind the scene object that implements the `HookManager` interface, that provides method definitions for action and filter handling. The provided HookManager implementation of this interface is of course based on the WordPress core concepts and uses the `add_action` and `add_filter` functions. 

The HookManager is registered inside the container with the `wwp.hook.manager` key, which means you can replace it with your own implementation if you want.

The `AbstractHookService` class then retrieves the HookManager from inside the container and keeps a private reference to it. the `addAction` and `addFilter` methods then delegate directly the hook handling to the referenced HookManager. That's how your own hook service and the HookManager can communicate.
