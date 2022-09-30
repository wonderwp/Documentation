# Getting Started

## 1) Create the theme files and folders

### With the generator

The easiest way to start creating a theme with WonderWp is to use the provided [generator](./02_Generator.md).

## 2) Register your theme autoloading

If you use a namespace for your theme classes (which is recommended), then you'll need to reference it in your composer.json file like so :

```
//Add your theme autoloading instruction inside your composer.json "autoload" section
    "autoload": {
        "psr-4": {
            //[...] Existing declarations ,
            "Your\\Theme\\Namespace\\": "your/theme/path/to/includes",
        }
    },
    
//For example, if your theme namespace is MyName\MyTheme, and its path is wp-content/themes/mytheme,
//You'd add this line in the autoload section : 

    "autoload": {
        "psr-4": {
            //[...] Existing declarations ,
            "MyName\\MyTheme\\": "wp-content/themes/mytheme/includes",
        }
    },  
```

Once you've added your autoloading instructions, rebuild composer autoloader with the following command : 

```
composer dump-autoload
```

If you forget, you'll likely get a PHP fatal error about your theme manager not being found.

## 3) Activate your theme

Activate your theme as you would normally do. (via the admin or with wp-cli)

## 4) Next steps

Now that your theme is activated, and the manager is running properly, the next step depends on what you need to do with your theme.

Usually it starts with the fact to allow the theme to interact with the project thanks to some sort of bridge between them. There are 3 sorts of such bridges : hooks, shortcodes, and routes.

- If you want to work with a hook :
    - If you do not have a [hook service](../02_Creating_a_plugin/04_Services/01_Hook_service.md) in your theme yet, create one.
    - If not done yet, register your hook service inside your manager.
    - Add your hook definition in the hook service register method.
    - Delegate the hook treatment to a dedicated service. (Create this service if it doesn't exist yet)

- If you want to work with a shortcode :
    - If you do not have a [shortcode service](../02_Creating_a_plugin/04_Services/06_Shortcode_service.md) in your theme yet, create one.
    - If not done yet, register your shortcode service inside your manager.
    - Add your shortcode definition in the shortcode service register method
    - Delegate the shortcode treatment to a dedicated service (or controller). (Create this service or controller if it doesn't exist yet)
    - As you can see the methodology is the same than the one with hooks. 

- If you want to work with a route :
    - If you do not have a [route service](../02_Creating_a_plugin/04_Services/02_Route_service.md) in your theme yet, create one.
    - If not done yet, register your route service inside your manager.
    - Add your route definition in the route service register method
    - Delegate the shortcode treatment to a dedicated service (or controller). (Create this service or controller if it doesn't exist yet)

The generator might have done most of the work for you.
