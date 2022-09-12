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

<a name="writing-service-provider"></a>
## Writing Service Provider

All service providers extend the `Syscodes\Components\Support\ServiceProvider` class. Most service providers contain a register and a boot method. For the sake of understanding how the above methods work, you should onñy bind things into the service container, that is, you should never try to register event listeners, routes, or any other function inside the register method.

<a name="register-method"></a>
### The Register Method

As mentioned previously, whitin the register method, you should only bind things into the service container. You should never attempt to register event listeners, routes, or any other functions within the register method. Otherwise, you may accidentally use a service provided that is provided by a service provider which has not loaded yet.

Within any of your service provider methods, you always have access to the `$app` property which provides access to the service container, like so:

    <?php

    namespace App\Providers;

    use App\Library\Services\DemoOne;
    use Syscodes\Components\Support\ServiceProvider;

    class CustomServiceProvider extends ServiceProvider
    {
        /**
         * Register any application services.
         *
         * @return void
         */
        public function register()
        {
            $this->app->singleton(DemoOne::class, function () {
                return (new DemoOne)->starting();
            });
        }
    }

> **Note**
> This service provider only defines a register method, and uses that method to define an implementation of `App\Library\Services\DemoOne` in the service container. If you're not yet familiar with Lenevor's service container, check out [its documentation](/docs/container).
