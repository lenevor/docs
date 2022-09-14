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
    - [Storing Items In The Cache]
    - [Removing Items From The Cache](#removing-items-cache)
    - [Cache Helper](#cache-helper)

<a name="introduction"></a>
## Introduction


When performing some data retrieval or processing tasks perfomed by your application may be CPU intensive or take several seconds to complete. If this is the case, it is most common to cache the retrieved data for some time so that it can be called quickly on subsequent requests for the same data.

Lenevor provides a unified and expressive API around some of the most popular forms of caching backends, allowing you blazing fast and dynamic caching of your application. Except for file based storage only, it requires specific server requirements, and a fatal exception will be thrown if the server requirements are not met.