# Repository Component

Repositories are a design pattern well known in frameworks. We though in WordPress they could have a place too, to gather several procedural functions use to retrieve posts under the hood of an interface.

The `PostRepository` class implements the given `RepositoryInterface`, which provides several method definitions as you can see on github.

## How to use the repository in your controller?

The best way is to register a reference of the repository inside your manager : 

```
public function register(Container $container)
{
    parent::register($container);
    
	//Public Controller
	$this->addController(AbstractManager::PUBLIC_CONTROLLER_TYPE, function () {
		return new MyPublicController($this); //Note that we inject $this so the controller has the manager reference
	});    
    
    $this->addService('repository', function () {
        return new PostRepository();
    });    
    
}
```

Then retrieve the repository inside your controller action :

```
public function myControllerAction(){
	//Retrieve repository
	$repo = $this->manager->getService('respository');
	
	//Interrogate repository to retrieve posts
	$posts = $repo->findAll();
	
	//Then pass them to the view
	$this->renderView('list',['posts'=>$posts]);
}
```

Some other interesting things with this approach is that in definitive, there's nn WordPress specific procedural code in this method, and also, the repository is injected, no instanciated directly. 

That means that you could write another `RepositoryInterface`implementation in place of the `PostRepository` class. Even one that is not a WordPress one at all, for third party api calls for example. And that wihtout touching anyting else than the line that registers the service in the manager.

