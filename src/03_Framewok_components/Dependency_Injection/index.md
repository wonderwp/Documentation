# Dependency Injection Component

Dependency injection is a pillar of service based architectures as it allows you to choose / swap and inject your services within your code without actually changing much of it. It's also a pillar of east oriented programming where you try to push to use of injected interface references instead of using hard coded class instances.

In order to some dependency injection capabilities in our code base, we introduced the Dependency Injection component, which is based on the pimple container.

In order to gain access to the container there's shorthand method : `$container = Container::getInstance()`, but we suggest to limit its usage to strategic places. Otherwise you might fall into the Service Locator Design Pattern instead of the Dependency injection one. Furthermore, the container is injected within your WonderWp Managers whom is the object responsible for registering your controllers and services. That means you can pass the container reference to them as well without having to reuse the shorthand method.

Typically the container holds the framework components references, and the managers references. If you have a look at the WonderWp Framework loader you'll see where it stores the framework component references into the container.

## The components keys stored in the container

- Cache : `wwp.cache.cache`
- Forms
	- Form object : `wwp.forms.form`. [Factory]
	- Form view : `wwp.forms.formView` [Factory]
	- Form validator : `wwp.forms.formValidator` [Factory]
- Log : `wwp.log.log`
- Mailers : `wwp.emails.mailer`	
- Router : `wwp.routes.router`

## How the container works

This is a pimple container, so whenever we want to figure out something about the container, we have a look at their [dedicated documentation](https://pimple.symfony.com/).

## Changing a component

If you want to change the mailer, the cache mechanism or any component reference really, you can redefine it from your own code. But please be advised it will be a global change.

For example, if you want to change the mailer that is being used, because let's say you need a mandrill mailer or a full blown smtp mailer, instead of the default mailer which is a wp_mail implementation of the MailerInterface. you would do it like so:

```
$container['wwp.emails.mailer'] = $container->factory(function () {
    return new MySuperMailer();
});
```

Then in your manager, register a service that would use the mailer reference from the container, not the class.

```
public function register(Container $container)
{
    parent::register($container);
    
    $this->addService('mailHandler', function () use($container) {
        return new ServiceThatNeedsToMailThings($container['wwp.emails.mailer']);
    });    
    
}
```

This way if you want to change the mailer in the future you'd just have to change the mailer reference again in the container without having to rechange neither your manager nor your service.