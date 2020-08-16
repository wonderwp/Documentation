# Public Controller

The aim of a public controller is to be the entry point of your frontend call stack. By default you'll enter this file either via a route (be it a file route or a callable route - see [the routing section](./06_Services/02_Route_service.md) for more information), or via a shortcode.

## Creating a public controller

There's an `AbstractPluginFrontendController` class you can extend to add a few functionalities to your own public frontend controller, but more precisely some default support for those two entry points types.

## Registering the controller within you manager

```
//Public Controller
$this->addController(AbstractManager::PUBLIC_CONTROLLER_TYPE, function () {
    return new MyPublicController($this); //Note that we inject $this so the controller has the manager reference
});
```

The fact to inject the manager into the public controller here will allow you to access the manager instance from within the controller, and therefore you'll have access to the registered config and services.

## Reaching the controller

You have two methods to reach your controller : via a shortcode or a route. The idea is to create a dedicated controller method (=an action) that's going to be called by either of the methods.

### ... via shortcode

- Inside your shortcode service : `add_shortcode('mypluginshortcode', [$myPluginFrontendController, 'handleShortCode']);`

### ... via a route

- See the [route service documentation](./06_Services/02_Route_service.md).
- TL;DR:
 
```
$this->_routes = [
	['recette/resetfilters/{previousRecettePageId}',array($manager->getController(AbstractManager::$PUBLICCONTROLLERTYPE),'resetFilters'),'GET'] //example of a route that maps a url to a callable
];
```

## Inside the action

Should have reached your controller action by then. Once inside your action, the idea is to sollicitate services to gather some variable values to pass to the view, for example:

```
    /**
     * @param array $attributes
     *
     * @return string
     */
    public function listAction(array $attributes = [])
    {
        $viewParams = [
            'mapper' => $this->manager->getConfig('entityViewMapper'),
            'links' => $this->manager->getService('links')->getLinksList($this->request),
            'showPagination' => !isset($attributes['limit']),
            'baseUrl' => '',
            'pagination' => $this->manager->getService('pgination')->getPagination($this->request),
            'filtersFormView' => $this->getService('filters')->getFiltersFormView()
        ];
        $viewContent = $this->renderView('linkslist' , $viewParams);
        return $viewContent;
    }
```

Services are registered towards your plugin manager and your manager is available from inside the controller under `$this->manager`.

You also have access tp the request instance under `$this->request`. 
That way you can get $_GET or $_POST values safely like this :

```
//Retrieve a $_GET value :
$a = $this->request->query->get('a');

//Retrieve a $_POST value :
$b = $this->request->request->get('b');
```

More on this subject in the [Http Foundation documentation section](../03_Framewok_components/Http_Foundation/index.md).

## Calling a view from inside a controller action

```
return $this->renderView(
    'myViewFileName', // The view name => myPluginRoot/public/views/myViewFileName.php
    [ // The view params
        'param1'  => 'val1', 
        'param2' => 'val2',
    ] //You can now use $param1 & $param2 inside myViewFileName.php
);
```

This tries to locate the `myViewFileName.php` inside your plugin `/public/views/` folder. Something like `myPluginRoot/public/views/myViewFileName.php`.

If the file is found, from within the file, you'll be able to access your view parameters. From the example they'll be respectively called $param1 (which will be equal to 'val1')  & $param2 (='val2');

You have also a similar method called `renderPage` that's going to create a WordPress post from scratch and inject it in the WP_Query object in order to create a fake post from nothing. This is especially usefull in the context of controller actions reached by routes.

## Example of a full public controller

```
<?php

namespace WonderWp\Plugin\Emploi\Controller;

use WonderWp\Plugin\Core\Framework\AbstractPlugin\AbstractPluginDoctrineFrontendController;
use WonderWp\Plugin\Emploi\Entity\Offre;
use WonderWp\Plugin\Emploi\Repository\OffreRepository;
use WonderWp\Plugin\Emploi\Service\EmploiCandidatureFormService;
use WonderWp\Plugin\Emploi\Service\EmploiFilterService;

class EmploiPublicController extends AbstractPluginDoctrineFrontendController
{

    /**
     * @param array $attributes
     *
     * @return string
     */
    public function listAction(array $attributes = [])
    {
    	 //Using $this->request to retrieve pagination parameters
        $page    = (int)$this->request->get('pageno', 1);
        $perPage = $this->manager->getConfig('offre_per_page', 20);
        $showPagination = !isset($attributes['limit']);

        //Get offre repository from manager. See PostRepository for the implementation
        /** @var OffreRepository $repository */
        $repository = $this->manager->getService('repository');

	     //We want to display some filters on this page
        /** @var EmploiFilterService $filterService */
        $filterService = $this->manager->getService('filters');
        $filtersForm   = $filterService->getFiltersForm($this->request->query->all());

        $criterias = $filterService->prepareCriterias($this->request, $attributes);
        
        //Query to get the posts
        $offres               = $repository->findBy($criterias, ['id' => 'DESC'], $perPage, ($page - 1) * $perPage);        

		 //Preparing view parameters
        $viewParams = [
            'mapCallable'         => $this->manager->getConfig('viewEntityMapper'),
            'showPagination'      => $showPagination,
            'baseUrl'             => '',
            'paginationComponent' => $this->container['wwp.theme.component.pagination'],
            'offres'              => $offres
        ];

		 //Showing the view
        $viewContent = $this->renderView($attributes['vue'], apply_filters('wwp.emploi.list.viewParams', $viewParams));

        return $viewContent;
    }

}
```
