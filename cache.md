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
    - [Redis Store](#redis-store)
    - [Null Store](#null-store)
- [Cache Usage](#cache-usage)
    - [Obstaining A Cache Instance](#obstaining-cache-instance)
    - [Retrieving Items From The Cache](#retrieving-items-cache)
    - [Storing Items In The Cache](#storing-items-cache)
    - [Removing Items From The Cache](#removing-items-cache)
    - [Cache Helper](#cache-helper)

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