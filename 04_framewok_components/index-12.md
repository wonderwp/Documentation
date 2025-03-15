# The Sanitizer Component

The sanitizer helps you sanitize request inputs.

## How to retrieve the sanitizer

The sanitizer is available as a service in the container under the `wwp.sanitizer` key.

```
$sanitizer = $container['wwp.sanitizer']; //Recommended
```

It is also available as a singleton that can be retrieved like this :

```
$sanitizer = WonderWp\Component\Sanitizer\Sanitizer::getInstance();
```

## How to use the sanitizer

The sanitizer has a sanitize method that takes an unsafe array as a parameter and returns a sanitized array.

```
use WonderWp\Component\HttpFoundation\Request;
use WonderWp\Component\Sanitizer\Sanitizer;

$request = Request::getInstance();
$sanitizer = Sanitizer::getInstance(); //or $sanitizer = $container['wwp.sanitizer'] if you have access to the container

$query = $request->query->all();
// $query is unsafe
$sanitizedQuery = $sanitizer->sanitize($inputs);
// $sanitizedQuery is safer
```

It also supports different WordPress sanitization via the following static functions :

- `sanitizeText`
- `sanitizeHtml`
- `sanitizeEmail`
- `sanitizeUrl`
- `sanitizeInteger`
- `sanitizeFloat`
- `sanitizeData`

Finally, it has a global `sanitize` function that will try to guess the type of the input and sanitize it accordingly.
