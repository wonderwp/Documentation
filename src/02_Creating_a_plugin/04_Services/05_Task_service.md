# Task Service (WP-CLI)

Sometimes in plugins, you could want to create some specific WP-CLI command.

## How to create a task service

Create a class that implements the `TaskServiceInterface`, which requires you to implement a `register` method.

```
class MyPluginCommandService implements TaskServiceInterface
{
    public function register(){
        if (!class_exists('WP_CLI') || !(defined( 'WP_CLI' ) && WP_CLI)) { return; }
        
        //Register your commands
        \WP_CLI::add_command( 'myCommandName', MyCommandClass::name ); //authorizing a new wp-cli task works by calling this method with a task name and a class to execute it
    }
}
```

## Registering your service

Register your service inside your plugin manager : 

```
$this->addService(ServiceInterface::COMMAND_SERVICE_NAME, function () {
    return new MyPluginCommandService();
});
```

## Launching your command

This should allow you to run : `vendor/bin/wp myCommandName`, and by doing so it will execute `MyCommandClass::_invoke(array $args, array $assocArgs)`.

See the [WP_CLI add_command documentation](https://make.wordpress.org/cli/handbook/references/internal-api/wp-cli-add-command/) for more information.
