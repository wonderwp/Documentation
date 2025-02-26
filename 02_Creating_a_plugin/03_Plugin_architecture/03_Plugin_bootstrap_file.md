
# The Plugin Bootstrap File

The bootstrap file is the file that is going to be loaded by WordPress in its **active_plugins** option. It is your plugin entry point.

Here's an example of a plugin bootstrap file, straight out of the generator, that we'll comment right after:

```
<?php

/**
 * The plugin bootstrap file
 *
 * This file is read by WordPress to generate the plugin information in the plugin
 * admin area. This file also includes all of the dependencies used by the plugin,
 * registers the activation and deactivation functions, and defines a function
 * that starts the plugin.
 *
 * @wordpress-plugin
 * 
 * Plugin Name: MyPlugin
 * Description: Online courses management
 * Version: 0.0.1
 * Text Domain: city
 * Domain Path: /languages
 */

use WonderWp\Component\PluginSkeleton\Service\ActivatorInterface;
use WonderWp\Component\PluginSkeleton\Service\DeactivatorInterface;
use WonderWp\Component\DependencyInjection\Container;
use WonderWp\Component\PluginSkeleton\ManagerInterface;
use WonderWp\Component\PluginSkeleton\Exception\ServiceNotFoundException;
use WonderWp\Component\Service\ServiceInterface;
use MyPlugin\MyPluginManager;

// If this file is called directly, abort.
if (!defined('WPINC')) {
    die;
}

define('WWP_MYPLUGIN_NAME', 'city');
define('WWP_MYPLUGIN_VERSION', '1.0.0');
define('WWP_MYPLUGIN_TEXTDOMAIN', 'city');
if (!defined('WWP_MYPLUGIN_MANAGER')) {
    define('WWP_MYPLUGIN_MANAGER', MyPluginManager::class);
}

/**
 * Register activation hook
 * The code that runs during plugin activation.
 */
register_activation_hook(__FILE__, function () {
    try {
        /** @var ActivatorInterface $activator */
        $activator = Container::getInstance()->offsetGet(WWP_MYPLUGIN_NAME . '.Manager')->getService(ServiceInterface::ACTIVATOR_NAME);
        $activator->activate();
    } catch (ServiceNotFoundException $e) {
        //This plugin hasn't registered any activator
    }
});

/**
 * Register deactivation hook
 * The code that runs during plugin deactivation.
 */
/*register_deactivation_hook(__FILE__, function () {
    try {
        /** @var DeactivatorInterface $deactivator *//*
        $deactivator = Container::getInstance()->offsetExists(WWP_MYPLUGIN_NAME . '.Manager') ? Container::getInstance()->offsetGet(WWP_MYPLUGIN_NAME . '.Manager')->getService(ServiceInterface::DEACTIVATOR_NAME) : null;
        $deactivator->deactivate();
    } catch (ServiceNotFoundException $e) {
        //This plugin hasn't registered a deactivator
    }
});*/

/**
 * The core plugin class that is used to define internationalization,
 * admin-specific hooks, and public-facing site hooks.
 * This class is called the manager
 * Instanciate here because it handles autoloading
 */
$plugin = WWP_MYPLUGIN_MANAGER;
$plugin = new $plugin(WWP_MYPLUGIN_NAME, WWP_MYPLUGIN_VERSION);

if (!$plugin instanceof ManagerInterface) {
    throw new \BadMethodCallException(sprintf('Invalid manager class for %s plugin : %s', WWP_MYPLUGIN_NAME, WWP_MYPLUGIN_MANAGER));
}

/**
 * Begins execution of the plugin.
 *
 * Since everything within the plugin is registered via hooks,
 * then kicking off the plugin from this point in the file does
 * not affect the page life cycle.
 *
 * @since    1.0.0
 */
$plugin->run();
```

Ideally, this file should do only a few things:

-  Define a few useful **constants** (plugin name, version and textdomain)
-  Register an **activation** hook
-  Register a **deactivation** hook
-  Require and instantiate the plugin **manager**

The rest of the plugin mechanic is now handled by the [manager](./04_Plugin_Manager.md), that you could discover next.
