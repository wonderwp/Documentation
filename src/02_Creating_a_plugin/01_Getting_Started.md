# Getting Started

## 1) Create the plugin files and folders

### With the generator

The easiest way to start creating a plugin with WonderWp is to use the provided [generator](./02_Generator.md).

### Or by hand

If you do not feel like using the command line, you can follow the following steps by hand.<br />
The idea is to manually create the [plugin architecture](./03_Plugin_architecture/02_File_architecture.md).<br />
It's longer to create al those files by hand (and the generator does a tremendous job to make sure you do not have to) :

- Create your plugin folder
- Create in it your plugin [bootstrap file](./03_Plugin_architecture/03_Plugin_bootstrap_file.md)
- Optionally, add next to it an index.php file with the silence is golden comment in it.
- Create an "includes" folder
- Create in it your [plugin manager](./03_Plugin_architecture/04_Plugin_Manager.md)

This is the minimal setup for a plugin to run properly, and the generator generates a more complete plugin architecture.

## 2) Register your plugin autoloading

If you use a namespace for your plugin classes (which is recommended), then you'll need to reference it in your composer.json file like so :

```
//Add your plugin autoloading instruction inside your composer.json "autoload" section
    "autoload": {
        "psr-4": {
            //[...] Existing declarations ,
            "Your\\Plugin\\Textdomain\\": "your/plugin/path/to/includes",
        }
    },
    
//For example, if your plugin namespace is MyName\MyPlugin, and its path is wp-content/plugins/myplugin,
//You'd add this line in the autoload section : 

    "autoload": {
        "psr-4": {
            //[...] Existing declarations ,
            "MyName\\MyPlugin\\": "wp-content/plugins/myplugin/includes",
        }
    },  
```

Once you've added your autoloading instructions, rebuild composer autoloader with the following command : 

```
composer dump-autoload
```

If you forget, you'll likely get a PHP fatal error about your plugin manager not being found.

## 3) Activate your plugin

Activate your plugin as you would normally do. (via the admin or with wp-cli)

## 4) Next steps

Now that your plugin is activated, and the manager is running properly, the next step depends on what you need to do with your plugin.

Usually it starts with the fact to allow the plugin to interact with the project thanks to some sort of bridge between them. There are 3 sorts of such bridges : hooks, shortcodes, and routes.

- If you want to work with a hook :
    - If you do not have a [hook service](./04_Services/01_Hook_service.md) in your plugin yet, create one.
    - If not done yet, register your hook service inside your manager.
    - Add your hook definition in the hook service register method.
    - Delegate the hook treatment to a dedicated service. (Create this service if it doesn't exist yet)

- If you want to work with a shortcode :
    - If you do not have a [shortcode service](./04_Services/06_Shortcode_service.md) in your plugin yet, create one.
    - If not done yet, register your shortcode service inside your manager.
    - Add your shortcode definition in the shortcode service register method
    - Delegate the shortcode treatment to a dedicated service (or controller). (Create this service or controller if it doesn't exist yet)
    - As you can see the methodology is the same than the one with hooks. 

- If you want to work with a route :
    - If you do not have a [route service](./04_Services/02_Route_service.md) in your plugin yet, create one.
    - If not done yet, register your route service inside your manager.
    - Add your route definition in the route service register method
    - Delegate the shortcode treatment to a dedicated service (or controller). (Create this service or controller if it doesn't exist yet)

The generator might have done most of the work for you.

For other types of work to do, head over more advanced documentation topics such as :

- [Plugin architecture](./03_Plugin_architecture)
- [Plugin services](./04_Services)
- [Framework capabilities](../03_Framewok_components)
