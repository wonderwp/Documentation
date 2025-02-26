# Admin Controller

When you use the generator to create your plugin, it will create an admin controller in its folder architecture for you.

It's not used unless you'd like to create an admin space for your plugin in the WordPress admin.

## Activating your admin controller

If you head over your hook service, you'll see a line like this :

```
//add_action('admin_menu', [$this, 'customizeMenus']); //If you want to add a link to your plugin in the admin menu, uncomment this then modify the customizeMenus method
```

You can un comment this line, that will then call this method that you should see further down your hook service : 

```
    /**
     * Add plugin menu entry in admin menu
     */
    public function customizeMenus()
    {
        //Get admin controller
        $adminController = $this->manager->getController(AbstractManager::ADMIN_CONTROLLER_TYPE);
        $callable        = [$adminController, 'route'];

        //Add entry in admin menu
        add_menu_page('MyPluginName', 'MyPluginName', 'edit_posts', WWP_PLUGIN_STATE_NAME, $callable);
    }
```

You can modify the  `add_menu_page` parameters to change the way your plugin menu entry is shown main admin menu. 

You should now see it by now, and should be able to click on it.

## Default action

By default, if you do not modify your admin controller and click your plugin entry in the main admin menu, you should land on the default admin action, that displays a message like this :

```
This is your admin controller defaultAction() method.
You should override it in Your\Plugin\NameSpace\YourPluginAdminController to display your own admin content.
```

That's something you can do : override the `defaultAction()` method in your own plugin controller to change what you want to show on your page.

## Tabs

You can override the `getTabs()` method to specify tabs for your plugin.

This method needs to return an array like so : 

```
    /** @inheritdoc */
    public function getTabs()
    {
        $tabs = [
            1 => ['action' => 'listRecipes', 'libelle' => 'Recipes'],
            2 => ['action' => 'listIngredients', 'libelle' => 'Ingredients'],
        ];

        return $tabs;
    }
```

This will create two tabs:

- The first one will show a "Recipes" label, and a link to your plugin page, with a $_GET parameter action set to "listRecipes".
- The second one will show a "Ingredients" label, and a link to your plugin page, with a $_GET parameter action set to "listIngredients".

## Actions

To handle the two actions defined in your tabs, you need to create actions in your admin controller : the `listRecipesAction` method and the `listIngredients` one.

```
public function listRecipesAction(){
    //handle your admin recipes there
}

public function listIngredients(){
    //handle your admin ingredients there
}
```
