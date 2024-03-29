# Route Service

Manipulating the WP_Rewrite API is not an easy task in WordPress. We've tried to abstract the complexity of creating custom routes for your plugin thanks to the use of a route service.

With a route service, you can define two kinds of routes:

- Routes that map a url to a certain file
- Routes that map a url to a certain callable

## How to create a route service
Create a class that extends the `AbstractRouteService` class. The `AbstractRouteService` class implements the `RouteServiceInterface`, therefore it requires that your route service implements a `getRoutes()` method.

```
class MyPluginRouteService extends AbstractRouteService
{
    public function getRoutes(){
        if(empty($this->routes)) {
            $manager = $this->manager; //Here we get the plugin manager to access the plugin controller, which will be used as a callable object.
            $this->routes = array(
                ['(.*)/arome/{arome}/typeplat/{typeplat}/instant/{instant}/pageno/{pageno}','index.php?pagename=$matches[1]&arome=$matches[2]&typeplat=$matches[3]&instant=$matches[4]&pageno=$matches[5]','GET'], //example of a route that maps a url to a file
                ['recette/resetfilters/{previousRecettePageId}',array($manager->getController(AbstractManager::$PUBLICCONTROLLERTYPE),'resetFilters'),'GET'] //example of a route that maps a url to a callable
        }

        return $this->routes;
    }
}
```

## Registering the route service
Add these few lines inside your plugin manager.

```
$this->addService(ServiceInterface::ROUTE_SERVICE_NAME,function(){
    //Hook service
    return new MyPluginRouteService();
});
```

## More on routes
See the [dedicated doc section](../../03_Framewok_components/03_Routing/index.md).
