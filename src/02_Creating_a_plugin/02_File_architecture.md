# Plugin file architecture



Here's the main architecture guideline so you can get a feeling about how to organize your files and folders, and see where to put your code.

This is a guideline. Your plugin could have much less files (just the includes and admin folders for example).

- admin //All related admin resources
    - views
        - my-plugin-view-a.php
	- css
		- my-plugin-admin.css
	- js
		- my-plugin-admin.js 
- includes //All the autoloaded classes
	-  Controller
		- AdminController.php //Backend Controller
		- PublicController.php //FrontEnd Controller
    - [...]
	-  Services
		- HookService.php
		- Activator.php //Activation routine (table creation)
		- Deactivator.php //Deactivation routine (if any)
		- [...]
   - Manager.php //Main entry point
- languages //language files
    - myplugin.pot, .mo, .po
- public //All related public resources
	- css 
		- my-plugin-public.css
	- js
		- my-plugin-public.js  
	- views
- index.php //Silence is golden
- myplugin.php //Plugin bootstrap file


Now that you've had a better view of the plugin architecture, let's then get into more details about what you have in the includes folder.