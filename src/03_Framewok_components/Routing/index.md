# The Routing Component

Hands up if you ever pulled your hair while trying to register custom routing rules in a WordPress site. ✋ We certainly did.

Registering custom WordPress routes is a combination of hooks you need to setup properly to add the rule, get the query variables right, flush the routes to have yours recognised, but not every time to avoid an expensive operation slowing down your site.

As you can see, it could be quite complex to get it right and efficient all the time. We took the time to dive deep into the subject to extract the complexity out of it and propose an implementation of a Routing system that would do its best to help the developers.

## The Router

The router's job is to gather your custom routes, then feed them to the WordPress routing hooks so you don't have to. 

The router is the central object that register the crucial hooks concerned about routing by default, and branches them on its own protected methods.

It is injected in the container under the `wwp.routing.router` key. That means you can replace it with your own if you want to. It is an object that's beeing used by the framework internally, so you don't have to instanciate it yourself, nor call inside your controllers for example. To declare your own routes, we encourage you to make use of routing services.

The router then asks each routing service it knows for custom routes, and takes it from there.

## Declaring custom routes

There's a [dedicated section](../../02_Creating_a_plugin/06_Services/02_Route_service.md) in the documentation for this that we encourage you to read beacause that's where you'll see how to declare your own routes.