# Service Providers

- [Introduction](#introduction)
- [Writing Service Provider](#writing-service-provider)
    - [The Register Method](#register-method)
    - [The Boot Method](#boot-method)
- [Registering Providers](#registering-providers)
- [Register Deferred Providers](#register-deferred-providers)

<a name="introduction"></a>
## Introduction

Service Providers are the central all Lenevor application bootstrapping. Even your own application, as well as all of Lenevor's core services, are bootstrapping through service providers.

However, what do you mean by "bootstrapped applications"? In general, we mean registering anything, including registering service binding, event listeners, middleware, and even routes.

If you open the `config/services.php` file included with Lenevor, you will see a `providers` array. In it you find all the classes of service providers that will be loaded for your application. By default, a set of Lenevor Core Service Providers is included in this array. These providers bootstrap the core components of Lenevor, such as the cache, database, events, views, and others. Also, many of these providers are "diferred" providers, meaning they will not be loaded on every request, but only when the services being called are actually needed.

It is important to clarify, that you will learn to write your own service providers and register them with your Lenevor application.

<a name="writing-service-provider"></a>
## Writing Service Provider

All service providers extend the `Syscodes\Components\Support\ServiceProvider` class. Most service providers contain a `register` and a `boot` method. For the sake of understanding how the above methods work, you should onñy bind things into the service container, that is, you should never try to register event listeners, routes, or any other function inside the `register` method.

<a name="register-method"></a>
### The Register Method

As mentioned previously, whitin the register method, you should only bind things into the [service container](/docs/{{version}}/container). You should never attempt to register event listeners, routes, or any other functions within the `register` method. Otherwise, you may accidentally use a service provided that is provided by a service provider which has not loaded yet.

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
> This service provider only defines a register method, and uses that method to define an implementation of `App\Library\Services\DemoOne` in the service container. If you're not yet familiar with Lenevor's service container, check out [its documentation](/docs/{{version}}/container).

<a name="boot-method"></a>
### The Boot Method

The `boot` method is where you can extend the basic functionality of Lenevor. In this method, you can access all the services that were registered using the service provider's registration method. For example, if you want to register "parameters" of a route, you must do it in the `boot` method, using regular expressions you may filter all the parameters when they are going to be used in a specific route, in the following:

    <?php

    namespace App\Providers;

    use Syscodes\Components\Support\ServiceProvider;

    class ParameterServiceProvider extends ServiceProvider
    {
        /**
         * Bootstrap any application services.
         *
         * @return void
         */
        public function boot()
        {
            Route::pattern('id', '[0-9]+');
        }
    }

#### Boot Method Dependency Injection

You may type-hint dependencies for your service provider's `boot` method. The [service container](/docs/{{version}}/container) will automatically inject any dependencies you need:

    use Syscodes\Components\Contracts\Routing\RouteResponse;

    public function boot(RouteResponse $response)
    {
        $response->macro('items', function ($value) {
            //
        });
    }

<a name="registering-providers"></a>
### Registering Providers

All service providers are registered in the `config/services.php` configuration file. This file contains a `providers` array where you can list the class names of your service providers. By default, a set of Lenevor core service providers are listed in this array. These providers bootstrap the core Lenevor components, such as the event listeners, database, cache, and others.

To register your provider, add it to the array:

    'providers' => [
        // Other service providers
        
        App\Providers\CustomServiceProvider::class,
    ],

<a name="register-deferred-providers"></a>
### Register Deferred Providers

If your provider is only registering bindings in the [service container](/docs/{{version}}/container), you may choose to defer its registration until one of the registered bindings is actually needed. Deferring the loading of such a provider will improve the performance of your application, since it is not loaded from the filesystem on every request.

Lenevor compiles and stores a list of all of the services supplied by deferred service providers, along with the name of their service provider class. Then, only when it tries to resolve one of these services, Lenevor load the service provider.

To be able to defer the loading a provider, implement the `Syscodes\Components\Contracts\Support\Deferrable` interface and define a `provides` method. This method called `provides` should return the service container bindings registered by the provider, as follows:

    <?php

    namespace App\Providers;

    use App\Library\Services\DemoOne;
    use Syscodes\Components\Support\ServiceProvider;
    use Syscodes\Components\Contracts\Support\Deferrable;

    class CustomServiceProvider extends ServiceProvider implements Deferrable
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

        /**
         * Get the services provided by the provider.
         *
         * @return array
         */
        public function provides(): array
        {
            return [DemoOne::class];
        }
    }