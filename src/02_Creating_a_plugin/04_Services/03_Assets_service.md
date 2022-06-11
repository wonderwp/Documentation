# Assets Service

The asset service is a place where you can register your assets. Registration is different from enqueuing. In this service, they are just registered so you can use them later on.

## How to create an asset service

Create a class that extends the `AbstractAssetService` class. `AbstractAssetService ` implements the `AssetServiceInterface `, therefore it requires that your hook service implements a `getAssets()` method.

```
class MyPluginAssetService extends AbstractAssetService
{
	public function getAssets(){
		//Register your assets from there
        if(empty($this->assets)) {
            $manager    = $this->manager;
            $pluginPath = $manager->getConfig('path.url');
            $assetGroup = 'plugins';
            /** @var Asset $assetClass */
            $assetClass   = self::$assetClassName;
            
            $this->assets = array(
                'css' => array(
                    new $assetClass('wwp-recette-admin', $pluginPath . '/admin/css/recette.scss', array('styleguide'), null, false, AbstractAssetService::$ADMINASSETSGROUP),
                    new $assetClass('wwp-recette', $pluginPath . '/public/css/recipe.scss', array('styleguide'), null, false, $assetGroup)
                ),
                'js' => array(
                    new $assetClass('wwp-recette-admin', $pluginPath . '/admin/js/recette.js', array('jquery-chosen'), null, false, AbstractAssetService::$ADMINASSETSGROUP),
                    new $assetClass('wwp-recette', $pluginPath . '/public/js/recette.js', array('app','styleguide'), null, true, $assetGroup)
                )
            );
        }
        return $this->assets;		
	}
}
```

## Registering the asset service

Add these few lines inside your plugin manager

```
$this->addService(ServiceInterface::ASSETS_SERVICE_NAME,function(){
    //Hook service
    return new MyPluginHookService();
});
```
