# The CPT repository

This file follows the [repository interface](https://github.com/wonderwp/Repository/blob/develop/src/RepositoryInterface.php) implementation and extends the [PostRepository](https://github.com/wonderwp/Repository/blob/develop/src/PostRepository.php) object.

It allows you to make queries to get your custom posts from the database.

#### Examples

##### Getting the repository from the manager 

```
/** @var CityRepository */
$repository = $this->manager->getService(ServiceInterface::ServiceInterface::REPOSITORY_SERVICE_NAME);
```

##### Getting every post

```
$cities = $repository->findAll();
```

##### Getting one post

```
$cityOne = $repository->find(1);
$cityTwo = $repository->find(2);
```

##### Making a specific request

The criterias array is the same than the one you would pass to [WP_Query](https://developer.wordpress.org/reference/classes/wp_query/#parameters) 
```
$criterias = [
  'author' => 123
];
$cities = $repository->findBy($criterias);
```
