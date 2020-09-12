# Getting Started

## Create the plugin files and folders

### With the generator
The easiest way to start creating a plugin with WonderWp is to use the provided [generator](./03_Generator.md).

### By hand
If you do not feel like using the command line, you can follow this steps by hand but it's longer : 

- Create your plugin folder
- Create in it your plugin [bootstrap file](./01_Understanding_the_WonderWp_philosophy/03_Plugin_bootstrap_file.md)
- Optionally, add next to it an index.php file with the silence is golden comment in it.
- Create an "includes" folder
- Create in it your [plugin manager](./01_Understanding_the_WonderWp_philosophy/04_Plugin_Manager.md)

This is the minimal setup for a plugin to run properly.

## Register your plugin autoloading

If you use a namespace for your plugin classes (which is recommended), then you'll need to reference it in your composer.json file like so :

```
```

## Next steps

Now that your plugin is activated and the manager is running properly, the next step depends on what you need to do with your plugin:

- If you wan to work with a hook :
    - If you do not have a hook service in your plugin yet, create one.
    - Add your hook definition in the hook service register method
    - Delegate the hook treatment to a dedicated service. (Create this service if it doesn't exist yet)
    
- If you want to work with a shortcode : 
     - If you do not have a shortcode service in your plugin yet, create one.
     - Add your shortcode definition in the shortcode service register method
     - Delegate the shortcode treatment to a dedicated service (or controller). (Create this service or controller if it doesn't exist yet)
