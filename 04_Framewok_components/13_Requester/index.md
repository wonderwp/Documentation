# The HTTP Component

The HTTP component is the component that helps you deal with HTTP requests and responses.

## The Requester

You can use a requester to make HTTP requests to external services.

Requesters should implement the `WonderWp\Component\Http\RequesterInterface` and provide implementations for the `get`, `post`, `put`, `delete`, `head` and `options` methods.

The framework provides a default implementation of the requester that you can use. It is available as a service in the container under the `wwp.http.requester` key.
It is based on WordPress http methods and uses the `wp_remote_*` functions to make the requests.
It returns a `WonderWp\Component\Http\Response` object that you can use to get the response body, headers, status code, etc.

```
/** @var \WonderWp\Component\Http\RequesterInterface $requester */
/** By default it is a \WonderWp\Component\Http\WpRequester instance */
$requester = $container['wwp.http.requester'];

$response = $requester->get('http://wonderwp.net/');
```