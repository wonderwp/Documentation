# Installation

## Prepare your WordPress install for Composer

WonderWp relies on composer to run properly. We'll therefore make sure that our WordPress install has a correct composer.json file that will hold the composer configuration.

If you don't have a composer.json file already, you could create one at the root of your project with the following content :

```
{
	"name":"authorname/projectname",
	"description":"Testing a WordPress project with wonderwp capabilities",
	"extra": {
		"installer-paths": {
			"wp-content\/mu-plugins\/{$name}\/": [
				"type:wordpress-muplugin"
			],
			"wp-content\/plugins\/{$name}\/": [
				"type:wordpress-plugin"
			],
			"wp-content\/themes\/{$name}\/": [
				"type:wordpress-theme"
			]
		},
		"wordpress-install-dir": "/"
	}
}
``` 

If you do have one already, what's important is to add the **extra** section to yours. 

Please adjust the folder paths to match your own installation if necessary.

Then require the composer/installer package : 

```
composer require composer/installers
```

That way, composer is now able to tell where your WordPress instance is located, and where it needs to install the packages it gets from its packagist mirror. 

You are now ready to install vendors, WordPress plugins, themes and must use plugins via composer.


## Prepare your WordPress install for must-use plugins (muplugins)

muplugins are some special kinds of plugins that can't be deactivated. If your WordPress install doesn't support them yet, you can activate them like so : [https://wordpress.org/support/article/must-use-plugins/](https://wordpress.org/support/article/must-use-plugins/) 


## Require WonderWp

This is a composer package, you can get it like this:

```
composer require wonderwp/wonderwp
```

After the command finished running, you should have the following folders :

- A wonderwp folder inside your vendors folder (vendor/wonderwp by default)
- An autoload-wwp folder in your must use plugins folder (wp-content/mu-plugins/autoload-wwp by default)
- An generator-wwp folder in your must use plugins folder (wp-content/mu-plugins/generator-wwp by default)

If that's not the case you might have a problem with your composer configuration (especially the installer paths).

## Initialisation

To be initialized, the WonderWp Framework loader should be called.
By default, WonderWp does so via its must use plugin called autoload-wwp.

The problem is that according to the must use plugins doc at the time of writing :

"WordPress only looks for PHP files right inside the mu-plugins directory, and (unlike for normal plugins) not for files in subdirectories. You may want to create a proxy PHP loader file inside the mu-plugins directory"

We've created such proxy file for you. It's called wonderwp-mu-proxy and it's inside the autoload-wwp folder. Copy it, then paste it in the mu-plugins folder.

## Testing if WonderWp is properly loaded

If your must use plugins work properly, you should see a dedicated section in your admin area (under plugins). Usually located at /wp-admin/plugins.php?plugin_status=mustuse

There you shoud see a list table of active must use plugins, and among this lit, you should see WonderWp.