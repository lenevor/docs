# Middleware

- [Introduction](#introduction)
- [Defining Middleware](#defining-middleware)
- [Registering Middleware](#registering-middleware)
    - [Global Middleware](#global-middleware)
    - [Assigning Middleware To Routes](#assigning-middleware-routes)
    - [Middleware Groups](#middleware-groups)
- [Middleware Parameters](#middleware-parameters)

<a name="introduction"></a>
## Introduction

Middleware provide a mechanism inspecting and filtering HTTP requests entering your application. All of middlewares are localted in the `app/Http/Middleware` directory. 

<a name="defining-middleware"></a>
## Defining Middleware

To create a middleware, you must do it (manually for now) from the `app/Http/Middleware` directory, as follows:

    <?php

    namespace App\Http\Middleware;

    use Closure;

    class [Middleware-Name]
    {
        /**
         * Handle an incoming request.
         * 
         * @param  \Syscodes\Http\Request  $request
         * @param  \Closure  $next
         * 
         * @return mixed
         */
        public function handle($request, Closure $next)
        {
            return $next($request);
        }
    }

> {Note} When used [Middleware-Name] in the example, it is for you to change the name of the middleware that you consider necessary for your application. 

<a name="middleware-responses"></a>
#### Middleware & Responses

Actually, a middleware can perform a task before or after passing the request to the application. For example, the following middleware would perform some task before the application uses the request: 

    <?php

    namespace App\Http\Middleware;

    use Closure;

    class [Before-Middleware]
    {
        public function handle($request, Closure $next)
        {
            // Perform action

            return $next($request);
        }
    }

> {Note} When used [Before-Middleware] in the example, it is for you to change the name of the middleware that you consider necessary for your application. 

Therefore, the middleware following would perform its task after the application uses the request: 

    <?php

    namespace App\Http\Middleware;

    use Closure;

    class [After-Middleware]
    {
        public function handle($request, Closure $next)
        {
            $response = $next($request);
            
            // Perform action

            return $response;            
        }
    }

> {Note} When used [After-Middleware] in the example, it is for you to change the name of the middleware that you consider necessary for your application.

<a name="registering-middleware"></a>
## Registering Middleware

<a name="global-middleware"></a>
### Global Middleware

If you want a middleware to run during every HTTP request to your application, you must list the middleware class in the `$middleware` property of your `app/Http/Lenevor.php` class. 

<a name="assigning-middleware-routes"></a>
### Assigning Middleware Routes

If you want to assign a middleware to specific routes, you must first assign the middleware a key in your application's `app/Http/Lenevor.php` file. By default, the `$routeMiddleware` property of this class contains the entries for the middleware. You may add your own middleware to this list and assign it a key of your choosing, as follows:

    // Within App\Http\Lenevor class...

    protected $routeMiddleware = [
        //
    ];

> {Note} The possible middlewares for the framework has not yet been created to by default.

Once the middleware has been defined in the HTTP Lenevor, you may use the `middleware` method to assign middleware to a route, as follows:

    Route::get('/user, function () {
        //
    })->middleware(['Middleware-Name']);

You may assign multiple middleware to the route by passing an array of middleware names to the middleware method, as follows:

    Route::get('/user, function () {
        //
    })->middleware(['Middleware-Name-1', 'Middleware-Name-2']);

When assigning middleware, you may also pass the fully qualified class name, as follows:

    use App\Http\Middleware\[Middleware-Name];

    Route::get('/user, function () {
        //
    })->middleware([Middleware-Name]::class);

<a name="middleware-groups"></a>
### Middleware Groups

You may want to group several middleware under a single key to make it easier to assign them to routes. You can do this using the `$middlewareGroups` property of your HTTP Lenevor.

Lenevor contains two groups of middleware called `web` and `api` that contain common middleware allowing to apply to your web and api routes. Remember, the service provider `App\Provider\RouteServiceProvider` of your application is in charge of automatically applying these middleware groups to the routes within corresponding the route `web` and `api` route files, as follows: 

    /**
     * Get the application's middleware groups.
     * 
     * @var array $middlewareGroups
     */
    protected $middlewareGroups = [
        'web' => [
            //
        ],

        'api' => [
            //
        ]
    ];

> {Note} A middleware has not yet been created, in future deliveries of the framework, the corresponding middleware will be published to filter routes, requests, events, security at the level of sending and receiving data, etc. 

Middleware groups may be assigned to routes and controller actions using the same syntax as individual middleware, as follows:

    Route::get('/', function () {
        //
    })->middleware('web');
    
    Route::middleware(['web'])->group(function () {
        //
    });

<a name="middleware-parameters"></a>
## Middleware Parameters

Middleware can also receive additional parameters. Additional middleware parameters will be passed to the middleware after the $next argument, as follows:

    <?php

    namespace App\Http\Middleware;

    use Closure;

    class [Middleware-Name]
    {
        /**
         * Handle an incoming request.
         * 
         * @param  \Syscodes\Http\Request  $request
         * @param  \Closure  $next
         * @param  string  $language
         * 
         * @return mixed
         */
        public function handle($request, Closure $next, $language)
        {
            if ($request->setLocale($language)) {
                // redirect
            }

            return $next($request);
        }
    }

Middleware parameters may be specified when defining the route by separating the middleware name and parameters with a `:`. Multiple parameters should be delimited by commas:

    Route::put('/user/{id}', function ($id) {
        //
    }->middleware('role:guest');