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
    - [Cache Helper](#cache-helper)

<a name="introduction"></a>
## Introduction


When performing some data retrieval or processing tasks perfomed by your application may be CPU intensive or take several seconds to complete. If this is the case, it is most common to cache the retrieved data for some time so that it can be called quickly on subsequent requests for the same data.

Lenevor provides a unified and expressive API around some of the most popular forms of caching backends, allowing you blazing fast and dynamic caching of your application. Except for file based storage only, it requires specific server requirements, and a fatal exception will be thrown if the server requirements are not met.

<a name="configuration"></a>
## Configuration

Your application's cache configuration file is located at `config/cache.php`. Since this file, you may specify which cache driver you would like to be used by default throughout your application. Lenevor comes ready with a number of caching backends such as [Memcached](https://memcached.org), [Redis](https://redis.io), and connection to relational databases. In addition, a file based cache driver is available, while array and "null" cache drivers provide cache backends to be used for automated tests.

Also the cache configuration file contains several other options, which are documented in the same file, so I invite you to read these configuration options. By default, Lenevor is configured to use the file cache driver, which stores serialized objects on the server's file system. When processing to building larger applications, it is recommended to use a more robust driver, such as Memcached or Redis.

