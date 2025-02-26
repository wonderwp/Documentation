# Logging Component

With the logging component we wanted to structure the logging strategy. That's why the component includes a logger interface based on the [PSR-3](https://www.php-fig.org/psr/psr-3/#3-psrlogloggerinterface) recommendation.

This component also provides 4 implementations of the interface: 
- The `DirectOutputLogger` class which logs to the standard output. 
- The `FileLogger` class, which logs to a file.
- The `VoidLogger` class, which logs into the void, output nothing, but is still a class that implements the interface properly if you need such object.
- The `WpCliLogger` class, which delegated all its logging method to corresponding [WP_CLI](https://make.wordpress.org/cli/handbook/references/internal-api/wp-cli-log/) ones 

## Changing the logger


```
$container['wwp.log.log'] = $container->factory(function () {
    return new DirectOutputLogger(); //Or any logger that implements the PSR-3 interface
});
```

## Using the logging component

In your manager, register a service that would use the logger reference from the container, not the class.

public function register(Container $container)
{
    parent::register($container);
    
    $this->addService('mailHandler', function () use($container) {
        return new ServiceThatNeedsToLogThings($container['wwp.log.log']);
    });   
    
}

This will automatically change the logger used in your service if you redefine the logger in the container. 

Then in your service you have access to a class that implements the [logger interface](https://www.php-fig.org/psr/psr-3/#3-psrlogloggerinterface). In the interface you'll see a full list of available methods.

### Example

```
$logger = $this->logger;
$logger->log('test log');
$logger->info('test info');
$logger->error('test error');
//etc
```
