# The Assets component

A WonderWp package allowing you to work with assets. It helps you declare them, enqueue them, and work with external systems such as builders (webpack, gulp...) if needed.

## Declaring assets

- Declaring assets works with Assets Services. Documentation about Assets Services can be found [here](../../02_Creating_a_plugin/04_Services/03_Assets_service.md).

## Declaring an enqueueur

- Using your assets within your theme works with Assets Enqueuers.
- You can decide which enqueuer you want to store in the container ('wwp.asset.enqueuer') based on your assets enqueuing strategy.
- By default, wonderwp works with the `DirectAssetEnqueuer`, which is basically a gateway of synthetic sugar or traditionnal WordPress methods.
- You have two other enqueuers available if you want : the `JsonAssetEnqueuer` and the `PackageAssetEnqueuer`, which are more meant to be used with external tooling such as gulp or webpack.
- You can also code your own. It must implement the [AssetEnqueuerInterface](https://github.com/wonderwp/Asset/blob/develop/src/AssetEnqueuerInterface.php).

#### The DirectAssetEnqueuer declaration

This is the framework default enqueuer.

It declaration looks like this : 

```
$container['wwp.asset.enqueuer']      = function ($container) {
    $publicPath = ROOT_DIR . str_replace('.', '', $container['wwp.asset.folder.prefix']);
    return new DirectAssetEnqueuer($container['wwp.asset.manager'], $container['wwp.fileSystem'], $publicPath);
}
```

#### The JsonAssetEnqueuer declaration

If you prefer to use a `JsonAssetEnqueuer`, you can redeclare the `wwp.asset.enqueuer` key in the container.

````
$container['wwp.asset.enqueuer']      = function ($container) {
    $fileVersion = $publicPath . $container['wwp.asset.folder.dest'] . '/version.php';
    $version = $fileSystem->exists($fileVersion) ? include($fileVersion) : null;
    
    return new JsonAssetEnqueuer(
        $container['wwp.asset.manager'],
        $fileSystem,
        $container['wwp.asset.manifest.path'],
        $publicPath,
        get_home_url(),
        $version,
    );
}
````


#### The PackageAssetEnqueuer declaration

If you prefer to use a `PackageAssetEnqueuer`, you can redeclare the `wwp.asset.enqueuer` key in the container.

````
$container['wwp.asset.enqueuer']      = function ($container) { 
    $fileSystem = $container['wwp.fileSystem'];
    $publicPath = ROOT_DIR . str_replace('.', '', $container['wwp.asset.folder.prefix']);
    $fullpath = ROOT_DIR . str_replace('.', '', $container['wwp.asset.folder.prefix']) . $container['wwp.asset.folder.dest'];
    
    $legacyPackage = new AssetPackage(
        new JsonManifestVersionStrategy($fullpath . '/legacy/manifest.json'),
        [
            'baseUrl' => get_home_url(),
            'basePath' => '/app/themes/wwp_child_theme/assets/final/legacy',
        ],
    );
    
    $modernPackage = new AssetPackage(
        new JsonManifestVersionStrategy($fullpath . '/modern/manifest.json'),
        [
            'baseUrl' => get_home_url(),
            'basePath' => '/app/themes/wwp_child_theme/assets/final/modern',
        ],
    );
    
    $packages = new AssetPackages(
        $legacyPackage,
        ['modern' => $modernPackage]
    );
    
    return new PackageAssetEnqueuer(
        $container['wwp.asset.manager'],
        $fileSystem,
        $packages,
        $publicPath,
    );
}
````

## Enqueuing assets

Principle : get the enqueuer from the container, ask him to enqueue assets as you whish.
<br />There are 16 methods available in the [AssetEnqueuerInterface](https://github.com/wonderwp/Asset/blob/develop/src/AssetEnqueuerInterface.php).

```
//Get assets enqueuer from the container
$container           = Container::getInstance();
$assetsEnqueuer = $container['wwp.asset.enqueuer'];
//Based on what you've defined in your container, it can be a 

//Ask him to enqueue assets as per defined in the AssetEnqueuerInterface interface
$assetsEnqueuer->enqueueScript($singleScriptHandleName);
$assetsEnqueuer->enqueueScripts([$singleScriptHandleName1,$singleScriptHandleName2]);
$assetsEnqueuer->enqueueStyle($singleStyleHandleName);
$assetsEnqueuer->enqueueStyles([$singleStyleHandleName1,$singleStyleHandleName2]);
//etc
```
