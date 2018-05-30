# The Cache Component

Caching seems to not be a core WordPress concept as you can see from the official codex page on the subject : [https://codex.wordpress.org/WordPress_Optimization/Caching](https://codex.wordpress.org/WordPress_Optimization/Caching).

You can read there about caching plugins that would serve your pages a static files.

## Transients

There's however a built in transient mechanism : [https://codex.wordpress.org/Transients_API](https://codex.wordpress.org/Transients_API) , which offers a simple and standardized way of storing cached data in the database temporarily.

The transient API declares several standalone functions : 

```
- set_transient()
- get_transient()
- set_site_transient()
- get_site_transient()
- delete_transient()
- delete_site_transient()
```

## WonderWp Cache Component

The cache component aims to provide a cache interface based on the PHP-FIG recommendations ([PSR6](https://www.php-fig.org/psr/psr-6/) & [PSR16](https://www.php-fig.org/psr/psr-16/)), then a set of implementations.

For now there's one interface (the PSR16 simple cache) provided and two implementations provided : the `DisabledCache` class and the `TransientCache` one.

The `DisabledCache` class allows you to disable your cache layer without modifying your code.

The `TransientCache` class organises the transient functions calls under the class that implements the simple cache interface.

Thanks to the interface, and the container, you could switch your caching strategy by injection either the `DisabledCache` class, or the `TransientCache` class, or any other caching class implementing the simple cache interface into the container under the `wwp.cache.cache` key.

## Examples

Choosing  the cache strategy by injection a caching class in the container under the `wwp.cache.cache` key :

```
$container['wwp.cache.cache'] = function () {
    return new TransientCache();
};
```

Using the injected cache class in you code : 

```
/** @var CacheInterface $cache */
$cache = $this->container['wwp.cache.cache'];
if (empty(($json = $cache->get('meteo.openweather.json')))) {

    // It wasn't there, so regenerate the data and save the transient
    $json = json_decode(utf8_encode(@file_get_contents($this->url)));
    if (empty($json)) {
        throw new \Exception('Unable to load meteo json from ' . $this->url);
    }

    $cache->set('meteo.openweather.json', $json, 1 * HOUR_IN_SECONDS);
}
```
