# The API Component

Defining ajax entry points for your plugin can feel a bit messy in a sense that it works with hooks. Those hooks could be defined at multiple places (unless you create a hook service), and for each hook defined, you associate a callable that can be a function, an object method somewhere, or somewhere else.

The API component is the component that tries to bring some order into this by providing an abstract class to help you create API services.

The idea behind the Api Service is to provide a class that will act as an API controller to gather all the API logic, and that can be used to abstract and ease ajax endpoints registration. 

All the public methods defined in this service can be used as an ajax endpoint via the Basic ajax calls via the old admin-ajax endpoint (wp-admin/admin-ajax.php). No more hook definition, you just create the API service, and then declare public methods in it. Those methods can now be accessed via JavaScript calls.
You can also annotate your methods to make them available in the new REST API.

Most of the concepts underlying the API component are described on the [API service page](../../02_Creating_a_plugin/04_Services/04_Api_service.md). You'll get plenty of details there.



