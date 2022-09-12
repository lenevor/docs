# Service Providers

- [Introduction](#introduction)
- [Writing Service Provider](#writing-service-provider)
    - [The Register Method](#register-method)
    - [The Boot Method](#boot-method)
- [Registering Providers](#registering-providers)
- [Register Deferred Providers](#Register-deferred-providers)

<a name="introduction"></a>
## Introduction

Service Providers are the central all Lenevor application bootstrapping. Even your own application, as well as all of Lenevor's core services, are bootstrapping through service providers.

However, what do you mean by "bootstrapped applications"? In general, we mean registering anything, including registering service binding, event listeners, middleware, and even routes.

If you open the `config/services.php` file included with Lenevor, you will see a `providers` array. In it you find all the classes of service providers that will be loaded for your application. By default, a set of Lenevor Core Service Providers is included in this array. These providers bootstrap the core components of Lenevor, such as the cache, database, events, views, and others. Also, many of these providers are "diferred" providers, meaning they will not be loaded on every request, but only when the services being called are actually needed.

It is important to clarify, that you will learn to write your own service providers and register them with your Lenevor application.