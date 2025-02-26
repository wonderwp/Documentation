# Plugin Activator 

Create a class that extends `AbstractPluginActivator` or implements the `ActivatorInterface`

```
class RecetteActivator extends AbstractPluginActivator
{

    /**
     * Create table for entity
     */
    public function activate()
    {
    	//What's inside the activate method will be executed upon plugin activation
    	//Here you could for example create a table for your plugin, define options, copy default language files...
      
    }
}
```
What's inside the activate method will be executed upon plugin activation
Here you could for example create a table for your plugin, define options, copy default language files...

## Registering the activator service
Now that your activator service is ready, add these few lines inside your plugin manager to let him now about it.

```
//Activator
$this->addService(ServiceInterface::ACTIVATOR_NAME, function () {
    return new MyPluginActivator($this->getVersion());
});
```

Also, make sure the following lines are present in your plugin [bootstrap file](../03_Plugin_architecture/03_Plugin_bootstrap_file.md) : 

```
/**
 * Register activation hook
 * The code that runs during plugin activation.
 */
register_activation_hook(__FILE__, function () {
    try {
        /** @var ActivatorInterface $activator */
        $activator = Container::getInstance()->offsetGet(WWP_PLUGIN_NAME . '.Manager')->getService(ServiceInterface::ACTIVATOR_NAME);
        $activator->activate();
    } catch (ServiceNotFoundException $e) {
        //This plugin hasn't registered any activator
    }
});
```
