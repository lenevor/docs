# Cache

- [Introduction](#introduction)
- [Configuration](#configuration)
    - [Prerequisites](#prerequisites)
- [Drivers Cache](#drivers-cache)
    - [Apc Store](#apc-store)
    - [Array Store](#array-store)
    - [Database Store](#database-store)
    - [File Store](#file-store)
    - [Memcached Store](#memcached-Store)
    - [Null Store](#null-store)
    - [Redis Store](#redis-store)
- [Cache Usage](#cache-usage)
    - [Obstaining A Cache Instance](#obstaining-cache-instance)
    - [Retrieving Items From The Cache](#retrieving-items-cache)
    - [Storing Items In The Cache](#storing-items-cache)
    - [Removing Items From The Cache](#removing-items-cache)
    - [The Cache Helper](#cache-helper)
- [Adding Custom Cache Drivers](#adding-custom-cache-drivers)
    - [Writing The Driver](#writing-the-driver)
    - [Registering The Driver](#registering-the-driver)

<a name="introduction"></a>
## Introduction

When performing some data retrieval or processing tasks perfomed by your application may be CPU intensive or take several seconds to complete. If this is the case, it is most common to cache the retrieved data for some time so that it can be called quickly on subsequent requests for the same data.

Lenevor provides a unified and expressive API around some of the most popular forms of caching backends, allowing you blazing fast and dynamic caching of your application. Except for file based storage only, it requires specific server requirements, and a fatal exception will be thrown if the server requirements are not met.

<a name="configuration"></a>
## Configuration

Your application's cache configuration file is located at `config/cache.php`. Since this file, you may specify which cache driver you would like to be used by default throughout your application. Lenevor comes ready with a number of caching backends such as [Memcached](https://memcached.org), [Redis](https://redis.io), and connection to relational databases. In addition, a file based cache driver is available, while `array` and "null" cache drivers provide cache backends to be used for automated tests.

Also the cache configuration file contains several other options, which are documented in the same file, so I invite you to read these configuration options. By default, Lenevor is configured to use the `file` cache driver, which stores serialized objects on the server's file system. When processing to building larger applications, it is recommended to use a more robust driver, such as Memcached or Redis.

<a name="prerequisites"></a>
### Prerequisites

#### Database

When using the `database` cache driver, you will create and setup a table to contain the cache items. An example `Schema` declaration is specified for the table below:

    Schema::table('cache', function ($table) {
        $table->string('key')->unique();
        $table->text('value');
        $table->integer('expiration');
    });

#### Memcached

Using the `Memcached` driver it requires the installation of the [Memcached PECL package](https://pecl.php.net/package/memcached). You may list all of your Memcached servers in the `config/cache.php` configuration file. From this file contains the `memcached.servers` entry to get you started:

    'memcached' => [
        'servers' => [
            [
                'host' => env('MEMCACHED_HOST', '127.0.0.1'),
                'port' => env('MEMCACHED_PORT', 11211),
                'weight' => 100,
            ],
        ],
    ],

#### Redis

Should use a `Redis` cache with Lenevor, you will need to install the PhpRedis PHP extension via PECL or install the `predis/predis` package (~1.0) via Composer. You may modified in the configuration file `config/cache.php` directly in `redis.driver` and `redis.connection` options:

    'redis' => [
        'driver' => 'redis',
        'connection' => 'cache',
    ],

<a name="drivers-cache"></a>
## Drivers Cache

<a name="apc-store"></a>
### Apc Store

Lenevor can using `APC` for your cache system that is used to store compiled PHP code and user data, which allows the web server, to process a greater number of requests per second, however it is convenient to point out that a configuration wrong use of it can slow down the response process, so it is important to adjust parameters until you find the right performance. You also have the option of using the `APCu` cache as long as you have this extension installed on your server, thereby using shared memory on the web server to store objects. This makes it very fast and capable of providing atomic read/write functions.

<a name="array-store"></a>
### Array Store

Stores all data in an array. This engine does not provide persistent storage and is intended for use in application test suites.

<a name="database-store"></a>
### Database Store

All setup are specified via a table in the database you use for your application. Therefore, the information is stored in that table and everything depends on the browser cache.

<a name="file-store"></a>
### File Store

The file based caching allows for pieces of view files or of compilation to be cached. Use this with care, and make sure to benchmark your application, as a point can come where disk I/O will negate positive gains by caching. This requires a cache directory to be really writable by the application. Also, this is the default caching by Lenevor.

<a name="memcached-store"></a>
### Memcached Store

Memcached allows you to store the results returned from a query to the database, but in addition to this data, you may store any information that comes to mind, for example calculation results, user session information, etc.

<a name="null-store"></a>
### Null Store

This is a caching backend that will always 'miss'. It stores no data, but lets you keep your caching code in place in environments that don't support your chosen cache.

<a name="redis-store"></a>
### Redis Store

Redis using an in-memory key-value store which may operate in cache mode. To use it, you need Redis server and phpredis PHP extension.

Modify the config options to connect to redis server stored  in the cache configuration file to `config/cache.php`.

<a name="cache-usage"></a>
## Cache Usage

<a name="obstaining-cache-instance"></a>
### Obstaining A Cache Instance

To obtain a cache store instance, you may use the `Cache` facade, which is the most used throughout this documentation. The `Cache` facade allows access to implementations Lenevor cache contracts:

    <?php

    namespace App\Http;

    use Syscodes\Components\Support\Facades\Cache;

    class ProductController extends Controller
    {
        /**
         * Show a list of all products.
         *
         * @return \Syscodes\Components\Http\Response
         */
        public function index()
        {
            $value = Cache::get('key');
            
            //
        }
    }

<a name="accessing-multiple-cache-stores"></a>
#### Accessing Multiple Cache Stores

At using the `Cache` facade, you may access various `cache` stores through the store method. Which, the key passed to the `store`  should correspond to one of the `stores` listed in the `stores` configuration array in your `cache` configuration file, as follow:

    $value = Cache::store('file')->get('key');

    Cache::store('memcached')->put('key', 'Good product!', 600); // 10 Minutes

<a name="retrieving-items-cache"></a>
### Retrieving Items From The Cache

The `Cache` facade's `get` method is used to retrieve items from the cache. If the item does not exist in the cache,`null` will be returned. If you wish, you may pass a second argument to the `get` method specifying the default value you wish to be returned if the item doesn't exist:

    $value = Cache::get('key');

    $value = Cache::get('key', 'default');

You may also pass a closure as the default value. The result of the closure will be returned if the item does not exist in the cache. Passing a closure allows you to defer retrieval of default values from a database or other external services, as follows:

    $value = Cache::get('key', function () {
        return DB::table(/* ... */)->get();
    });

<a name="checking-item-existence"></a>
#### Checking For Item Existence

The `has` method may be used to determine if an item exists in the cache. This method may return a `false` value if the item exists, but its value is `null`, as follows:

    if (Cache::has('key')) {
        //
    }

<a name="incrementing-decrementing-values"></a>
#### Incrementing / Decrementing Values

The `increment` and `decrement` methods are used to adjust the value of integer items in the cache. These methods mentioned above accept an optional second argument to indicate by which to increment or decrement the item's value, as follows:

    Cache::increment('key');
    Cache::increment('key', $value);
    Cache::decrement('key');
    Cache::decrement('key', $value);

<a name="retrieve-delete"></a>
#### Retrieve & Delete

If you need to retrieve an item from the cache and then delete it, you may use the `pull` method. As with the `get` method, a `null` value will be returned if the item does not exist in the cache, as follows:

    $value = Cache::pull('key');

<a name="storing-items-cache"></a>
### Storing Items In The Cache

To store elements in the cache use the `put` method, as follows:

    Cache::put('key', 'value', 10); // Seconds = 10

If the storage time is not passed to the `put` method, the item will be stored indefinitely, as follow:

    Cache::put('key', 'value');

Also, you have the ability to passing the number of seconds as an integer or you may pass a `Datetime` instance representing the desired expiration time of the cached item, as follows:

    Cache::put('key', 'value', now()->addMinutes(10));

<a name="store-if-not-item-present"></a>
#### Store If  Not A Item Present

The `add` method will only add the item to the cache if it does not already exist in the cache store. The method will return `true` if the item is actually added to the cache. Otherwise, the method will return `false`, as follow:

    Cache::add('key', 'value', $seconds);

<a name="storing-items-forever"></a>
#### Storing Items Forever

The `forever` method may be used to store an item in the cache permanently. Since these items will not expire, they must be manually removed from the cache using the `delete` method, as follow:

    Cache::forever('key', 'value');

<a name="removing-items-cache"></a>
### Removing Items From The Cache

You may remove items from the cache using the `delete` method, as follow:

    Cache::delete('key');

You may also remove items by providing a zero or negative number of expiration seconds, as follow:

    Cache::put('key', 'value', 0);

    Cache::put('key', 'value', -5);

You may clear the entire cache using the flush method, as follow:

    Cache::flush();

<a name="cache-helper"></a>
### The Cache Helper

Also to using the `Cache` facade, you may also use the global `cache` function to retrieve and store data via the cache. When the `cache` function is called with a single, string argument, it will return the value of the given key, as follows:

    $value = cache('key');

When the cache function is called without any arguments, it returns an instance of the `Syscodes\Components\Contracts\Cache\Factory` implementation, allowing you to call other caching methods, as follow:

    cache()->put('key', 'value', 10);

    cache()->get('key');

<a name="adding-custom-cache-drivers"></a>
## Adding Custom Cache Drivers

<a name="writing-the-driver"></a>
### Writing The Driver

To create our custom cache driver, we first need to implement the `Syscodes\Components\Contracts\Cache\Store` [contract](/docs/{{version}}/contracts). So, a MongoDB cache implementation might look something as follow:

    <?php

    namespace App\Extensions;

    use Syscodes\Components\Contracts\Cache\Store;

    class MongoStore implements Store
    {
        public function get(string $key) {}
        public function many(array $keys): array {}
        public function put(string $key, mixed $value, int $seconds): bool {}
        public function putMany(array $values, int $seconds): bool {}
        public function increment(string $key, mixed $value = 1): int|bool {}
        public function decrement(string $key, mixed $value = 1): int|bool {}
        public function delete(string $key): mixed {}
        public function forever(string $key, mixed $value): bool {}
        public function flush(): bool {}
        public function getPrefix(): string {}
    }

We just need to implement each of these methods using a MongoDB connection. Once our implementation is complete, we can finish our custom driver registration by calling the `Cache` facade's `extend` method, as follow:

    Cache::extend('mongo', function (Application $app) {
        return Cache::repository(new MongoStore);
    });

> **Note**  
> If you're wondering where to put your custom cache driver code, you could create an `Extensions` namespace within your `app` directory. However, keep in mind that Lenevor does not have a rigid application structure and you are free to organize your application according to your preferences.

<a name="registered-the-driver"></a>
### Registered The Driver

To register the custom cache driver with Lenevor, we will use the `extend` method on the `Cache` facade. Since other service providers may attempt to read cached values within their `boot` method, we will register our custom driver within a `booting` callback. By using the `booting` callback, we can ensure that the custom driver is registered just before the `boot` method is called on our application's service providers but after the `register` method is called on all of the service providers. We will register our `booting` callback within the `register` method of our application's `App\Providers\AppServiceProvider` class, as follow:

    <?php
    
    namespace App\Providers;
    
    use App\Extensions\MongoStore;
    use Syscodes\Components\Support\Facades\Cache;
    use Syscodes\Components\Support\ServiceProvider;
    use Syscodes\Components\Contracts\Core\Application;
    
    class CacheServiceProvider extends ServiceProvider
    {
        /**
         * Register any application services.
         */
        public function register()
        {
            $this->app->booting(function () {
                Cache::extend('mongo', function (Application $app) {
                    return Cache::repository(new MongoStore);
                });
            });
        }
        
        /**
         * Bootstrap any application services.
         */
        public function boot()
        {
            // ...
        }
    }

The first argument passed to the `extend` method is the name of the driver. This will correspond to your `driver` option in the `config/cache.php` configuration file. The second argument is a closure that should return an `Syscodes\Components\Cache\CacheRepository` instance. The closure will be passed an `$app` instance, which is an instance of the [service container](/docs/{{version}}/container).

Once your extension is registered, update your `config/cache.php` configuration file's `driver` option to the name of your extension.