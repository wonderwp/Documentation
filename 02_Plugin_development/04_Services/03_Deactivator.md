# Plugin Deactivator

Create a class that implements the `DeactivatorInterface`.

```
class MyPluginDeactivator implements DeactivatorInterface{

	/**
	 * Short Description. (use period)
	 *
	 * Long Description.
	 *
	 * @since    1.0.0
	 */
	public function deactivate() {

	}

}
```
The code within the deactivate method will be executed upon plugin deactivation

## Registering the deactivator service
Now that your deactivator service is ready, add these few lines inside your plugin manager to let him now about it.

```
//Activator
$this->addService(ServiceInterface::DEACTIVATOR_NAME, function () {
    return new MyPluginDeactivator();
});
```

Also, make sure the following lines are present in your plugin [bootstrap file](../03_Plugin_architecture/03_Plugin_bootstrap_file.md) :

```
/**
 * Register deactivation hook
 * The code that runs during plugin deactivation.
 */
register_deactivation_hook(__FILE__, function () {
    try {
        /** @var DeactivatorInterface $deactivator *//*
        $deactivator = Container::getInstance()->offsetExists(WWP_PLUGIN_NAME . '.Manager') ? Container::getInstance()->offsetGet(WWP_PLUGIN_NAME . '.Manager')->getService(ServiceInterface::DEACTIVATOR_NAME) : null;
        $deactivator->deactivate();
    } catch (ServiceNotFoundException $e) {
        //This plugin hasn't registered any deactivator
    }
});
```
