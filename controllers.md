# Controllers

- [Introduction](#introduction)
- [Writing Controllers](#writing-controllers)
    - [Basic Controllers](#basic-controllers)
    - [Single Action Controllers](#single-action-controllers)
- [Resource Controllers](#resource-controllers)
    - [Partial Resource Routes](#restful-partial-resource-routes)
    - [Nested Resources](#restful-nested-resources)
    - [Naming Resource Routes](#restful-naming-resource-routes)
    - [Naming Resource Route Parameters](#restful-naming-resource-route-parameters)
    - [Scoping Resource Routes](#restful-scoping-resource-routes)
    - [Localizing Resource URIs](#restful-localizing-resource-uris)
    - [Supplementing Resource Controllers](#restful-supplementing-resource-controllers)
- [Dependency Injection & Controllers](#dependency-injection-and-controllers)

<a name="introduction"></a>
## Introduction

Instead of defining all of your request handling logic as closures in your route files, you may wish to organize this behavior using "controller" classes. Controllers can group related request handling logic into a single class. For example, a `ExampleController` class might handle the incoming requests related to a view. By default, controllers are stored in the `app/Http/Controllers` directory.

<a name="writing-controllers"></a>
## Writing Controllers

<a name="basic-controllers"></a>
### Basic Controllers

Let's take a look at an example of a basic controller. Note that the controller extends the base controller class included with Lenevor: `App\Http\Controller`:

    <?php

    namespace App\Http\Controllers;

    use App\Http\Controller;

    class ExampleController extends Controller
    {
        /**
         * Show the profile for a given user.
         *
         * @return \Syscodes\View\Factory
         */
        public function index()
        {
            return view('example', [
                'var' => 'Hello world!!'
            ]);
        }
    }

You can define a route to this controller method like so:

    use App\Http\Controllers\ExampleController;

    Route::get('/example', [ExampleController::class, 'index']);

When an incoming request matches the specified route URI, the `index` method on the `App\Http\Controllers\ExampleController` class will be invoked and the route parameters will be passed to the method.

<a name="single-action-controllers"></a>
### Single Action Controllers

If a controller action is particularly complex, you might find it convenient to dedicate an entire controller class to that single action. To accomplish this, you may define a single `__invoke` method within the controller:

    <?php

    namespace App\Http\Controllers;

    use App\Http\Controller;

    class Example2Server extends Controller
    {
        /**
         * Provision a new web server.
         *
         * @return \Syscodes\Http\Response
         */
        public function __invoke()
        {
            // ...
        }
    }

When registering routes for single action controllers, you do not need to specify a controller method. Instead, you may simply pass the name of the controller to the router:

    use App\Http\Controllers\Example2Server;

    Route::post('/server', Example2Server::class);

<a name="resource-controllers"></a>
## Resource Controllers

If you think of each Eloquent model in your application as a "resource", it is typical to perform the same sets of actions against each resource in your application. For example, imagine your application contains a `Photo` model and a `Movie` model. It is likely that users can create, read, update, or delete these resources.

The controller will contain a method for each of the available resource operations. Next, you may register a resource route that points to the controller:

    use App\Http\Controllers\PhotoController;

    Route::resource('photos', PhotoController::class);

This single route declaration creates multiple routes to handle a variety of actions on the resource. The generated controller will already have methods stubbed for each of these actions. 

You may even register many resource controllers at once by passing an array to the `resources` method:

    Route::resources([
        'photos' => PhotoController::class,
        'posts' => PostController::class,
    ]);

<a name="actions-handled-by-resource-controller"></a>
#### Actions Handled By Resource Controller

Verb      | URI                    | Action       | Route Name
----------|------------------------|--------------|---------------------
GET       | `/photos`              | index        | photos.index
GET       | `/photos/create`       | create       | photos.create
POST      | `/photos`              | store        | photos.store
GET       | `/photos/{photo}`      | show         | photos.show
GET       | `/photos/{photo}/edit` | edit         | photos.edit
PUT/PATCH | `/photos/{photo}`      | update       | photos.update
DELETE    | `/photos/{photo}`      | destroy      | photos.destroy

<a name="restful-partial-resource-routes"></a>
### Partial Resource Routes

When declaring a resource route, you may specify a subset of actions the controller should handle instead of the full set of default actions:

    use App\Http\Controllers\PhotoController;

    Route::resource('photos', PhotoController::class)->only([
        'index', 'show'
    ]);

    Route::resource('photos', PhotoController::class)->except([
        'create', 'store', 'update', 'destroy'
    ]);

<a name="api-resource-routes"></a>
#### API Resource Routes

When declaring resource routes that will be consumed by APIs, you will commonly want to exclude routes that present HTML templates such as `create` and `edit`. For convenience, you may use the `apiResource` method to automatically exclude these two routes:

    use App\Http\Controllers\PhotoController;

    Route::apiResource('photos', PhotoController::class);

You may register many API resource controllers at once by passing an array to the `apiResources` method:

    use App\Http\Controllers\PhotoController;
    use App\Http\Controllers\PostController;

    Route::apiResources([
        'photos' => PhotoController::class,
        'posts' => PostController::class,
    ]);

<a name="restful-nested-resources"></a>
### Nested Resources

Sometimes you may need to define routes to a nested resource. For example, a photo resource may have multiple comments that may be attached to the photo. To nest the resource controllers, you may use "dot" notation in your route declaration:

    use App\Http\Controllers\PhotoCommentController;

    Route::resource('photos.comments', PhotoCommentController::class);

This route will register a nested resource that may be accessed with URIs like the following:

    /photos/{photo}/comments/{comment}

<a name="restful-naming-resource-routes"></a>
### Naming Resource Routes

By default, all resource controller actions have a route name; however, you can override these names by passing a `names` array with your desired route names:

    use App\Http\Controllers\PhotoController;

    Route::resource('photos', PhotoController::class)->names([
        'create' => 'photos.build'
    ]);

<a name="restful-naming-resource-route-parameters"></a>
### Naming Resource Route Parameters

By default, `Route::resource` will create the route parameters for your resource routes based on the "singularized" version of the resource name. You can easily override this on a per resource basis using the `parameters` method. The array passed into the `parameters` method should be an associative array of resource names and parameter names:

    use App\Http\Controllers\AdminUserController;

    Route::resource('users', AdminUserController::class)->parameters([
        'users' => 'admin_user'
    ]);

 The example above generates the following URI for the resource's `show` route:

    /users/{admin_user}

<a name="restful-localizing-resource-uris"></a>
### Localizing Resource URIs

By default, `Route::resource` will create resource URIs using English verbs. If you need to localize the `create` and `edit` action verbs, you may use the `Route::resourceVerbs` method. This may be done at the beginning of the `boot` method within your application's `App\Providers\RouteServiceProvider`:

    /**
     * Define your route model bindings, pattern filters, etc.
     *
     * @return void
     */
    public function boot()
    {
        Route::resourceVerbs([
            'create' => 'crear',
            'edit' => 'editar',
        ]);

        // ...
    }

Once the verbs have been customized, a resource route registration such as `Route::resource('fotos', PhotoController::class)` will produce the following URIs:

    /fotos/crear

    /fotos/{foto}/editar

<a name="restful-supplementing-resource-controllers"></a>
### Supplementing Resource Controllers

If you need to add additional routes to a resource controller beyond the default set of resource routes, you should define those routes before your call to the `Route::resource` method; otherwise, the routes defined by the `resource` method may unintentionally take precedence over your supplemental routes:

    use App\Http\Controller\PhotoController;

    Route::get('/photos/popular', [PhotoController::class, 'popular']);
    Route::resource('photos', PhotoController::class);

> {tip} Remember to keep your controllers focused. If you find yourself routinely needing methods outside of the typical set of resource actions, consider splitting your controller into two, smaller controllers.

<a name="dependency-injection-and-controllers"></a>
## Dependency Injection & Controllers

<a name="constructor-injection"></a>
#### Constructor Injection

The service container is used to resolve all Lenevor controllers. As a result, you are able to type-hint any dependencies your controller may need in its constructor. The declared dependencies will automatically be resolved and injected into the controller instance:

    <?php

    namespace App\Http\Controllers;

    use App\Http\Controller;
    use App\Repositories\UserRepository;

    class UserController extends Controller
    {
        /**
         * The user repository instance.
         */
        protected $users;

        /**
         * Create a new controller instance.
         *
         * @param  \App\Repositories\UserRepository  $users
         * @return void
         */
        public function __construct(UserRepository $users)
        {
            $this->users = $users;
        }
    }

<a name="method-injection"></a>
#### Method Injection

In addition to constructor injection, you may also type-hint dependencies on your controller's methods. A common use-case for method injection is injecting the `Syscodes\Http\Request` instance into your controller methods:

    <?php

    namespace App\Http\Controllers;

    use Syscodes\Http\Request;

    class UserController extends Controller
    {
        /**
         * Store a new user.
         *
         * @param  Request  $request
         * @return Response
         */
        public function store(Request $request)
        {
            $name = $request->name;

            //
        }
    }

If your controller method is also expecting input from a route parameter, list your route arguments after your other dependencies. For example, if your route is defined like so:

    use App\Http\Controllers\UserController;

    Route::put('/user/{id}', [UserController::class, 'update']);

You may still type-hint the `Syscodes\Http\Request` and access your `id` parameter by defining your controller method as follows:

    <?php

    namespace App\Http\Controllers;

    use Syscodes\Http\Request;

    class UserController extends Controller
    {
        /**
         * Update the given user.
         *
         * @param  \Syscodes\Http\Request  $request
         * @param  string  $id
         * @return \Syscodes\Http\Response
         */
        public function update(Request $request, $id)
        {
            //
        }
    }