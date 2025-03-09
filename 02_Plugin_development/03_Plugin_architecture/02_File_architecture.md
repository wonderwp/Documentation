# Plugin file architecture



Here's the main plugin architecture guideline so you can get a feeling about how to organize your files and folders, and see where to put your code. You can also apply this structure to any theme you build with WonderWp.

This is a guideline. Your plugin could have much less files (just the includes and admin folders for example).

- **admin** //All related admin resources
- **includes** //All the autoloaded classes
    - [...]
	- [**BlockStyles**](../block-styles.md)
	- [**BlockTypes**](../block-types.md)
	- [**BlockVariations**](../block-variations.md)
	- [**Commands**](../commands.md)
	- [**CustomFields**](../custom-fields.md)
	- [**PostTypes**](../custom-post-types.md)
	- [**Taxonomies**](../taxonomies.md)
	- [**Hooks**](../hooks.md)
	- **Services**
		- HookService.php  ([See example](../04_Services/01_Hook_service.md))
		- Activator.php //Activation routine (table creation) ([See example](../04_Services/02_Activator.md))
		- Deactivator.php //Deactivation routine (if any) ([See example](../04_Services/03_Deactivator.md))
		- [...]
   - Manager.php //Main entry point  ([See example](./04_Plugin_Manager.md))
- **languages** //language files
    - myplugin.pot, .mo, .po
- **src** //All related public resources
- **tests** //Automated tests
	- [...]
- index.php //Silence is golden
- myplugin.php //Plugin bootstrap file  ([See example](./03_Plugin_bootstrap_file.md))


Now that you've had a better view of the plugin architecture, let's then get into more details about what you have in the includes folder.
