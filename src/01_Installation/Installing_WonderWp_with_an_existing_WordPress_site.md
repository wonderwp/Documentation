# Installing WonderWp on an existing WordPress install 

## Requirements

- PHP >= 8.3
- [Composer 2](https://getcomposer.org/)
- WordPress 7

## 1) Prepare your existing WordPress install for Composer

### 1.1) Check your composer.json file
WonderWp relies on composer to run properly. We'll therefore make sure that your WordPress install is able to run composer by checking if it has a correct composer.json file that holds the composer configuration.

- If you don't have a composer.json file already, create one manually at the root of your project with the following content :

```
{
	"name": "yourauthorname/yourprojectname",
	"description": "Testing a WordPress project with wonderwp capabilities",
	"type": "project",
	"config": {
		"allow-plugins": {
			"composer/installers": true
		}
	},
	"extra": {
		"installer-paths": {
			"wp-content/mu-plugins/{$name}/": [
				"type:wordpress-muplugin"
			],
			"wp-content/plugins/{$name}/": [
				"type:wordpress-plugin"
			],
			"wp-content/themes/{$name}/": [
				"type:wordpress-theme"
			]
		}
	}
}
``` 

Don't forget to replace `yourauthorname/yourprojectname` by your own values.

- If you do have one already, what's important is to add the **extra** section and the **allow-plugins** entry to yours. 

Why those keys:

- **installer-paths** tells composer where to put WordPress must-use plugins, plugins and themes. Please adjust the folder paths to match your own installation if necessary.
- **allow-plugins**: since Composer 2.2, a composer plugin such as `composer/installers` only runs if it is allowed. Without this entry, `composer require` stops with an error in non interactive mode (or asks you the question in interactive mode).

### 1.2) composer installer

Type this command to require the composer/installers package : 

```
composer require composer/installers
```

### 1.3) Composer is ready

After those two steps, composer knows where your WordPress instance is located, and where it needs to install the packages it gets from its packagist mirror. 

You are now ready to install vendors, WordPress plugins, themes and must use plugins via composer.

## 2) Require WonderWp

WonderWp is a composer package, you can get it like this:

```
composer require wonderwp/wonderwp
```

After the command finished running, you should have the following folders :

- A wonderwp folder inside your vendors folder (vendor/wonderwp by default)
- An autoload-wwp folder in your must use plugins folder (wp-content/mu-plugins/autoload-wwp by default)
- An generator-wwp folder in your must use plugins folder (wp-content/mu-plugins/generator-wwp by default)

If that's not the case you might have a problem with your composer configuration (especially the installer paths).

## 3) Initialize WonderWp

To be initialized, the WonderWp Framework loader should be called.
By default, WonderWp does so via its must use plugin called autoload-wwp.

The problem is that according to the must use plugins doc at the time of writing :

"WordPress only looks for PHP files right inside the mu-plugins directory, and (unlike for normal plugins) not for files in subdirectories. You may want to create a proxy PHP loader file inside the mu-plugins directory"

We've created such proxy file for you. It's called `wonderwp-mu-proxy.php` and it's inside the `mu-plugins/autoload-wwp` folder.<br /> 
Copy it, then paste it in the `mu-plugins` folder directly (in other words one level above its original position).

On a default WordPress install, you can do it with your terminal with the following command : 

```
cp wp-content/mu-plugins/autoload-wwp/wonderwp-mu-proxy.php wp-content/mu-plugins/
```

In your mu-plugins folder, you should then have at least the three following things : 
```
- mu-plugins/
    - autoload-wwp/
    - generator-wwp/
    - wonderwp-mu-proxy.php
```

## Testing if WonderWp is properly loaded

If your must use plugins work properly, you should see a dedicated section in your admin area (under plugins). Usually located at /wp-admin/plugins.php?plugin_status=mustuse (the tab is called "Must-Use" in English, "Indispensable" in French).

There you should see a list table of active must use plugins, and among this list, you should see WonderWp.

You can also check it with WP-CLI :

```
wp plugin list --status=must-use
```

If that's the case, your install should be ready to run with full modern development capabilities.
It's now time to [create your first plugin](../02_Creating_a_plugin/01_Getting_Started.md).

## Working on the framework itself

If you want to change the framework and see your changes right away in your WordPress site, install it from source instead of using a tagged release.

Add these keys to your composer.json :

```
"minimum-stability": "dev",
"prefer-stable": true,
"config": {
	"preferred-install": {
		"wonderwp/*": "source",
		"*": "dist"
	}
}
```

- **minimum-stability** / **prefer-stable**: a development version of WonderWp and of its components can only be installed with `minimum-stability: dev`. `prefer-stable: true` keeps every other package on stable releases.
- **preferred-install**: composer clones the WonderWp packages with git instead of downloading an archive.

Then require the development version :

```
composer require "wonderwp/wonderwp:dev-develop"
```

`vendor/wonderwp/wonderwp` (and every `vendor/wonderwp/*` component) is then a git repository: you can create a branch, change the code, see the result in your site right away, commit and push from there.

The framework's own unit tests run from a clone of the framework (`composer install` then `composer test`), see its README.
