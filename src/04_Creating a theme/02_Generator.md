# Generator

One purpose of the WonderWp framework is to allow you to create themes rapidly. To help you get started quickly, WonderWp embarks a theme generator as a wp-cli command. Invoking the command with a few parameters will generate for you the folder structure as well as the required files to kick start your theme development..

The good thing with a generator besides the fact that is makes you **gain time**, is that it ensures that the generated code follows the framework guidelines. It will create the bare **minimum folders and classes** necessary for the theme to work, then leaves you up and running, ready to **build on top** of its foundations.

## How to generate a theme? 

WonderWp provides a wp-cli command to help you generate your theme skeleton.

wp-cli must be installed. If it's not installed on your project, you can install it like this : `composer require wp-cli/wp-cli`

Then you can use the generator command `generate-theme` which you can invoke like so :

```
vendor/bin/wp generate-theme --name="mythemeName" --desc="This is my theme description" --namespace="WonderWp\theme\MythemeNameSpace"
```

As you can see it takes a few parameters based on which it generates the minimum files and folders:

- **name** : The name of your future theme - required
- **namespace** : Its php object namespace - required. You can provide any namespace, it doesn't have to start with WonderWp\theme
- **output_type** : Tell the generator which kind of theme you'd like to generate. Supported values for output_type are : 
  - "" - If you provide an empty value or don't provide this parameter, it will generate a default theme skeleton
  - "CPT" - If you specify CPT as the output_type, the generator will generate you a fully functional Custom Post Type theme
- Other WordPress theme header values are also supported : 
    - **desc** :  A short description of the theme, as displayed in the themes section in the WordPress Admin. Keep this description to fewer than 140 characters.
    - **uri** : The home page of the theme, which should be a unique URL, preferably on your own website. This must be unique to your theme. You cannot use a WordPress.org URL here.
    - **version** : The current version number of the theme, such as 1.0 or 1.0.3.
    - **licence** : The short name (slug) of the theme’s license (e.g. GPL2). More information about licensing can be found in the WordPress.org guidelines.
    - **licence_uri** : A link to the full text of the license (e.g. https://www.gnu.org/licenses/gpl-2.0.html).
    - **author** : The name of the theme author. Multiple authors may be listed using commas.
    - **author_uri** : The author’s website or profile on another website, such as WordPress.org.
    - **textdomain** : The gettext text domain of the theme. More information can be found in the Text Domain section of the How to Internationalize your theme page.
    - **domain_path** :   The domain path let WordPress know where to find the translations. More information can be found in the Domain Path section of the How to Internationalize your theme page.


After you generated your theme, make sure to register its autoloading in your composer.json file as explained in the [getting started section 2](./01_Getting_Started.md#page_2-Register-your-theme-autoloading)

### Examples

#### Generating a classic theme 

```
vendor/bin/wp generate-theme --name="mythemeName" --desc="This is my theme description" --namespace="WonderWp\theme\MythemeNameSpace"
```