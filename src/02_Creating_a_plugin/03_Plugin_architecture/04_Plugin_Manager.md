# The Plugin Manager

The manager is an essential file for your plugin. It's inside this file that you register three types of things :

- your **configurations**,
- your **controllers**, 
- your **services**.

## Creating your manager

The generator should have created the manager for you. If you want to create a manager by yourself, you need to follow these requirements :

- Extend the `AbstractManager` class or implement the `ManagerInterface` interface

## Registering your configs

To set a config for your plugin, you can use : `$this->setConfig($key,$val)`.

You can then retrieve the config value like this : `$this->getConfig($key,$defaultValueIfEmpty)`;

## Registering your controllers

You can register any type of controller you want. The most common (admin and public) have reserved constants on the `AbstractManager` class to help you enforce their type :

```
//Example of registering an Admin controller if you want ::
$this->addController(AbstractManager::ADMIN_CONTROLLER_TYPE, function () {
    return new ErThemeAdminController($this);
});

//Example of registering a public controller if you want ::
$this->addController(AbstractManager::PUBLIC_CONTROLLER_TYPE, function () {
    return new ErThemePublicController($this);
});

//Example of registering a custom (eg API) controller if you want ::
$this->addController('api', function () {
    return new ErThemePublicController($this);
});
```

## Registering your services

You can see the reserved service names as constants on the [ServiceInterface](https://github.com/wonderwp/Service/blob/develop/src/ServiceInterface.php) class.

Example of service registration:

```
$this->addService(ServiceInterface::HOOK_SERVICE_NAME, function () {
    //Hook service
    return new ErThemeHookService();
});
```
## Full example of a manager

```
<?php

namespace WonderWp\Plugin\Actu;

use WonderWp\Component\PluginSkeleton\AbstractManager;
use WonderWp\Component\PluginSkeleton\AbstractPluginManager;
use WonderWp\Component\DependencyInjection\Container;
use WonderWp\Component\Service\ServiceInterface;
use WonderWp\Plugin\Actu\Controller\ActuAdminController;
use WonderWp\Plugin\Actu\Controller\ActuPublicController;
use WonderWp\Plugin\Actu\Repository\ActuRepository;
use WonderWp\Plugin\Actu\Service\ActuActivator;
use WonderWp\Plugin\Actu\Service\ActuAssetsService;
use WonderWp\Plugin\Actu\Service\ActuFiltersService;
use WonderWp\Plugin\Actu\Service\ActuFrontMapper;
use WonderWp\Plugin\Actu\Service\ActuHookService;
use WonderWp\Plugin\Actu\Service\ActuPageSettingsService;
use WonderWp\Plugin\Actu\Service\ActuSearchService;
use WonderWp\Plugin\Core\Framework\PageSettings\AbstractPageSettingsService;

/**
 * Class ActuManager
 * @package WonderWp\Plugin\Actu
 * The manager is the file that registers everything your plugin is going to use / need.
 * It's the most important file for your plugin, the one that bootstraps everything.
 * The manager registers itself with the DI container, so you can retrieve it somewhere else and use its config / controllers / services
 */
class ActuManager extends AbstractPluginManager
{

    /**
     * Registers config, controllers, services etc usable by the plugin components
     *
     * @param Container $container
     *
     * @return $this
     */
    public function register(Container $container)
    {
        parent::register($container);

        //Register Config
        $this->setConfig('path.root', plugin_dir_path(dirname(__FILE__)));
        $this->setConfig('path.base', dirname(dirname(plugin_basename(__FILE__))));
        $this->setConfig('path.url', plugin_dir_url(dirname(__FILE__)));
        $this->setConfig('textDomain', WWP_ACTU_TEXTDOMAIN);

        $this->setConfig('news_per_page', $this->getConfig('news_per_page', 20));
        $this->setConfig('enableFilters', $this->getConfig('enableFilters', true));
        $this->setConfig('viewEntityMapper', $this->getConfig('viewEntityMapper', [ActuFrontMapper::class, 'map']));

        //Register Controllers
        $this->addController(AbstractManager::ADMIN_CONTROLLER_TYPE, function () {
            return new ActuAdminController($this);
        });
        $this->addController(AbstractManager::PUBLIC_CONTROLLER_TYPE, function () {
            return $plugin_public = new ActuPublicController($this);
        });

        //Register Services
        $this->addService(ServiceInterface::ACTIVATOR_NAME, function () {
            //Activator
            return new ActuActivator(WWP_PLUGIN_ACTU_VERSION);
        });
        $this->addService(ServiceInterface::HOOK_SERVICE_NAME, function () {
            //Hook service
            return new ActuHookService($this);
        });
        $this->addService(AbstractPageSettingsService::PAGE_SETTINGS_SERVICE_NAME, function () {
            //Page settings service
            return new ActuPageSettingsService();
        });
        $this->addService(ServiceInterface::SEARCH_SERVICE_NAME, function () {
            //Search service
            return new ActuSearchService();
        });
        $this->addService(ServiceInterface::ASSETS_SERVICE_NAME, function () {
            //Asset service
            return new ActuAssetsService($this);
        });
        $this->addService(ServiceInterface::REPOSITORY_SERVICE_NAME, function () {
            //Repository
            return new ActuRepository();
        });
        $this->addService('filters', function () {
            //Search service
            return new ActuFiltersService();
        });


        return $this;
    }

}
```
