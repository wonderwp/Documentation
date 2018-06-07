# Task Service (WP-CLI)

Sometimes in plugins, you could want to create some specific WP-CLI command.

## How to create a task service

Create a class that implements the `TaskServiceInterface`, which requires you to implement a `registerCommands` method.

```
class MyPluginCommandService implements TaskServiceInterface
{
    public function registerCommands(){
        if(!class_exists('WP_CLI')){ return; }
        
        //Register your commands
        \WP_CLI::add_command( 'commandName', CommandClass::name ); //authorizing a new wp-cli task works by calling this method with a task name and a class to execute it
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

As you can see, inside the registerCommand method, we call the `\WP_CLI::add_command` method to register our new command. The first parameter is the name of the command as you'll execute at some point with wp-cli, for example : `vendor/bin/wp commandName `. Upon execution, the command will instanciate the class you've provided by name in the second parameter. In our example, the CommandClass class.

This class needs to have an `__invoke` method that would kick the command execution.