# Shortcode Service

Similarly to hooks, in large plugins you could have multiple shortcode definitions, pointing to callables defined in various places. Still similarly to hooks, this service tries to act as the object
that gathers and manages the shortcode definitions.

## How to create a shortcode service

Create a class that extends the `AbstractShortcodeService` class. The `AbstractShortcodeService ` class implements the `ShortcodeServiceInterface`, therefore it requires that your route service
implements a `register()` method.

```
class MyPluginShortcodeService extends AbstractShortcodeService
{

    public function register()
    {
        //Add your shortcode declarations here
        add_shortcode('myshortcode', [$this, 'mySimpleShortCodeHandler']);
        add_shortcode('myshortcode2', [$this, 'myServiceShortcodeHandler']);
        add_shortcode('myshortcode3', [$this, 'myControllerHandlerMethod']);
    }
    
    //Simple shortcode handler that returns a value
    public function mySimpleShortCodeHandler(){
    	return 'hello world';
    }
    
    //For more advanced output computing, you could prefer to use a service
    public function myServiceShortcodeHandler($shortcodeAttributes){
        $myService = $this->manager->getService('shortcodeHandler');
        return $myService->handleShortcode($shortcodeAttributes);
    }
    
    //For even more advanced output, you could prefer to use a public controller
    public function myControllerHandlerMethod($shortcodeAttributes){
        $controller = $this->manager->getController(AbstractManager::PUBLIC_CONTROLLER_TYPE);
        return $controller->renderMyView($shortcodeAttributes);
    }    
}
```

## Registering the shortcode service

Add these few lines inside your plugin manager.

```
//Shortcode
$this->addService(ServiceInterface::SHORT_CODE_SERVICE_NAME, function(){
    return new MyPLuginShortcodeService();
});
```
