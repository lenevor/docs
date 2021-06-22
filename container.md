# Service Container

- [Introduction](#introduction)
    - [Inject Service Container In Route](#inject-service-container-route)
- [Binding](#binding)
    - [Simple Bindings](#simple-bindings)
    - [Binding A Singleton](#binding-singleton)
    - [Binding Instances](#binding-instances)
    - [Extending Bindings](#extending-bindings)
- [Resolving](#resolving)
    - [Make Method](#make-method)
    - [Automatic Injection](#automatic-injection)

<a name="introduction"></a>
## Introduction

The Lenevor service container is a powerful tool that helps organize and instantiate an application's objects, helping to standardize and centralize the way objects are structured in the application to emphasize class dependency management and injection dependency. The service container is characterized be easy , it is very fast and emphasizes an architecture that promotes reusable code. 

Let's look at a simple example:

    <?php

    namespace App\Http\Controllers;

    use App\Http\Controller;
    use Syscodes\Http\Request;

    class UserController extends Controller
    {
        /**
         * The HTTP Request implementation.
         * 
         * @var \Syscodes\Http\Request $request
         */
        protected $request;

        /**
         * Constructor. Create a new Controller instance.
         *
         * @param  \Syscodes\Http\Request  $request
         * 
         * @return void
         */
        public function __construct(Request $request)
        {
            $this->request = $request;
        }

        /**
         * Show the setlocale for given language.
         * 
         * @param  string  $locale
         *
         * @return \Syscodes\Http\Response
         */
        public function show($locale)
        {
            $lang = $this->request->setLocale($locale);

            return view('user.locale', ['lang' => $lang]);
        }
    }

In this example, the `UserController` needs to retrieve data from a language that the user chooses. So, we will inject a service that may recover the language data through `Request`.

Understanding of the Lenevor service container is essential to building a powerful and robust applications, as well as for contributing to Lenevor the core itself. 

<a name="inject-service-container-route"></a>
### Inject Service Container In Route

You will often type-hint  dependencies on route, controllers, event listeners, and anywhere in an application without ever manually interacting with the container. For example, you might type-hint the an instance of the object `Syscodes\Http\Request` in the definition of a route to easily access the current request. Even though we never have to interact with the container, as follows:

    use Syscodes\Http\Request;

    Route::get('/', function (Request $request) {
        // ...
    });

<a name="binding"></a>
## Binding

<a name="simple-bindings"></a>
### Simple Bindings

Almost always the container links of your service are registered within the service providers.

In the service provider, you have access to the container through the `$this->app` property. We may register a binding using the `bind` method, passing the name of the class or the interface name that is desired together with the closure that returns an instance of the class, as follows:

    use App\Services\Music;
    use App\Services\Parser;

    $this->app->bind(Music::class, function ($app) {
        return new Music($app->make(Parser::class));
    });

As mentioned above, it will typically be interacting with the container within service providers; However, if you would like to interact with the container outside of a service provider, you may do so via the `app` facade, as follows: 

    use App\Services\Music;
    use Syscodes\Support\Facades\App;

    App::bind(Music::class, function ($app) {
        // ...
    });

>{tip} For the classes it is not necessary to force them to have dependence on any interface in the container. By making it clearer, the container does not need to be instructed on how to build these objects, since it may automatically resolve these objects using the reflection.

<a name="binding-singleton"></a>
### Binding A Singleton

Which the `singleton` method bind a class or interface in the container that should be resolved only once. When once a singleton binding has been resolved, the same object instance will begin to return on subsequent calls into the container, as follows: 

    use App\Services\Music;
    use App\Services\Parser;

    $this->app->singleton(Music::class, function ($app) {
        return new Music($app->make(Parser::class));
    });

<a name="binding-instances"></a>
### Binding Instances

You may also bind an existing object instance into the container using the `instance` method. The instance method will always be returned on subsequent calls to the container, as follows: 

    use App\Services\Music;
    use App\Services\Parser;

    $this->app->instance(Music::class, new Parser);

<a name="extending-bindings"></a>
### Extending Bindings

The `extend` method allows modifying services that have been resolved. For example, when a service is resolved, you may run additional code to decorate or configure the service. The `extend` method accepts a closure and returns the modified service as its only argument. Therefore, the closure receives the service that is being resolved and the container instance, as follows: 

    $this->app->extend('Music::class', function ($service, $app) {
        return new DecoratedParser($service);
    });

<a name="resolving"></a>
## Resolving

<a name="make-method"></a>
### Make Method

The `make` method allows you to resolve a class instance from the container. This `make` method accepts the name of the class or interface you wish to resolve, as follows: 

    use App\Services\Music;

    $music = $this->app->make(Music::class);

If instead you are outside of a service provider in a location of your code where does not have access the `$this->app` variable, then you may use the `App` facade to resolve a class instance from the container, as follows: 

    use App\Services\Music;}
    use Syscodes\Support\Facades\App;

    $music = App::make(Music::class);

Also if you would the Lenevor container instance to be injected into a class that is being resolved by the container, you may specify the argument with the `Syscodes\Container\Container`class on your class constructor, as follows:

    use Syscodes\Container\Container;

    ...

    /**
     * The Container instance.
     *
     * @var \Syscodes\Container\Container $container
     */
    protected $container;

    /**
     * Constructor. Create a new class instance.
     *
     * @param  \Syscodes\Container\Container  $container
     *
     * @return void
     */
    public function __construct(Container $container)
    {
        $this->container = $container;
    }

<a name="automatic-injection">
### Automatic Injection

Importantly, you may type-hint the dependency on the constructor of a class that is resolved the container, including controllers, event listeners, middleware, and more. In practice, this is how most of your objects should be resolved by the container. As example, you may suggest in code that a class instance defined be in parameter from the controller's constructor. 

This class will be resolved and injected automatically into the class, as follows: 

    <?php

    namespace App\Http\Controllers;

    use App\Http\Controller;
    use Syscodes\Http\Request;

    class UserController extends Controller
    {
        /**
         * The HTTP Request implementation.
         * 
         * @var \Syscodes\Http\Request $request
         */
        protected $request;

        /**
         * Constructor. Create a new Controller instance.
         *
         * @param  \Syscodes\Http\Request  $request
         * 
         * @return void
         */
        public function __construct(Request $request)
        {
            $this->request = $request;
        }

        /**
         * Show the user with the given ID.
         * 
         * @param  string  $id
         *
         * @return \Syscodes\Http\Response
         */
        public function show($id)
        {
            //
        }
    }