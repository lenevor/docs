# Routing

- [Basic Routing](#basic-routing)
    - [Redirect Routes](#redirect-routes)
    - [View Routes](#view-routes)
- [Route Parameters](#route-parameters)
    - [Required Parameters]($required-parameters)
    - [Regular Expression Constraints](#regular-expression-constraints)
- [Named Routes](#named-routes)
- [Route Groups](#route-groups)
    - [Middleware](#middleware)
    - [Subdomain Routing](#subdomain-routing)
    - [Route Prefixes](#route-prefixes)
    - [Route Name Prefixes](#route-name-prefixes)
    
<a name="basic-routing"></a>
## Basic Routing

The most basic Lenevor routes accept a URI and a closure, providing a simple and expressive method of defining routes without complicated routing configuration files:

    use Syscodes\Support\Facades\Route;

    Route::get('/example', function() {
        return 'Hello world!!';
    });

<a name="the-default-route-files"></a>
#### The Default Route Files

All Lenevor paths are defined in their path files, which are located in the `routes` directory. These files are loaded automatically through the `App\Providers\RoutingServiceProvider` of your application. The `routes\web.php` file defines routes that are for the web interface and the routes in the `routes/api.php` file defines everything related to requests generated in data in `JSON` format.

Most applications will start by defining routes in their `routes/web.php` file, where these defined routes are accessed by entering the URL of the route defined in your browser. Here is an example, you can access the following route by navigating to `http://project-app.com/user` in your browser:

    use App\Http\Controllers\UserController;

    Route::get('/user', [UserController::class, 'index']);

Also, there is another way to call a controller and a method defined by you, it is simply to assign the character `@` in the text string in the file `routes/web.php` as follows:

    Route::get('/user', 'UserController@index');

The controller namespace is assigned in the `namespace` variable this desactived by default, you must uncomment this variable which is called in the `namespace` method of the `Route` facade in the `App\Providers\Routing` like so:

    $this->namespace = 'App\Http\Controllers';

    Route::middleware('web')
             ->namespace($this->namespace)
             ->group(basePath('routes/web.php'));

<a name="available-router-methods"></a>
#### Available Router Methods

The router allows you to register routes that respond to any HTTP verb:

    Route::get($uri, $callback);
    Route::post($uri, $callback);
    Route::put($uri, $callback);
    Route::patch($uri, $callback);
    Route::delete($uri, $callback);
    Route::options($uri, $callback);

Sometimes you may need to register a route that responds to multiple HTTP verbs. You may do so using the `match` method. Or, you may even register a route that responds to all HTTP verbs using the `any` method:

    Route::match(['get', 'post], function () {
        //
    });

    Route::any('/', function () {
        //
    });

<a name="dependency-injection"></a>
#### Dependency Injection

You have the option of using any dependencies required for a route route's callback signature. Therefore, all declared dependencies will be automatically resolved and injected into the callback by Lenevor's `service container`. For example, you can declare the class 'Syscodes\Http\Request' so that the current HTTP request is automatically injected into your route's callback: 

    use Syscodes\Http\Request;

    Route::get('/home, function (Request $request) {
        // ...
    });

<a name="redirect-routes"></a>
### Redirect Routes

If you are defining a route that redirects to another URI, you may use the Route::redirect method. This method provides a convenient shortcut so that you do not have to define a full route or controller for performing a simple redirect:

    Route::redirect('here', 'there');

By default, Route::redirect returns a 302 status code. You may customize the status code using the optional third parameter:

    Route::redirect('/here', '/there', 301);

> {note} When using route parameters in redirect routes, the following parameters are reserved by Lenevor and cannot be used: `destination` and `status`.

<a name="view-routes"></a>
### View Routes

If your route only needs to return a `view`, you may use the `Route::view` method. Like the `redirect` method, this method provides a simple shortcut so that you do not have to define a full route or controller. The `view` method accepts a URI as its first argument and a view name as its second argument. In addition, you may provide an array of data to pass to the view as an optional third argument:

    Route::view('/welcome', 'welcome');

    Route::view('/welcome', 'welcome', ['name' => 'Alexander']);

> {note} When using route parameters in view routes, the following parameters are reserved by Lenevor and cannot be used: `view`, `data`, `status`, and `headers`.

<a name="route-parameters"></a>
## Route Parameters

<a name="required-parameters"></a>
### Required Parameters

Sometimes you will need to capture segments of the URI within your route. For example, you may need to capture a user's ID form the URL. You may do so by defining route parameters:

    Route::get('/user/{id}, function ($id) {
        return 'user ID: '.$id;
    });

Route parameters are always enclosed in braces `{}` and must consist of alphanumeric characters. Underscores (`_`) are also accepted within the path parameters. Route parameters are injected into a route callbacks and the names of the route callback / controller arguments do not matter. 

<a name="dependency-injection"></a>
#### Dependency Injection

If your route has dependencies, Lenevor's service container to automatically inject into your route's callback. You must import it to be use of the class or interface you want to use as follows:

    use Syscodes\Http\Request;

    Route::get('/user/{id}', function (Request $request, $id) {
        return 'user ID: '.$id;
    });

<a name="regular-expression-constraints"></a>
### Regular Expression Constraints

You can constrain the format of your route parameters using the `where` method. This `where` method accepts the name of the parameter and a regular expression which defines how the parameter should be constrained: 

    Route::get('/user/{id}', function ($id) {
        return 'user ID: '.$id;
    })->where('id', '[0-9]+');

    Route::get('/user/{name}', function ($name) {
        return 'user Name: '.$name;
    })->where('name', '[a-zA-Z]+');

If the incoming request does not match the path regex constraints, a HTTP 404 response will be returned. 

<a name="global-constraints"></a>
#### Global Constraints

If you would like a route parameter to be defined by a pattern given globally for the entire framework, you may use the `pattern` method., you must define these patterns in the `boot` method of your `App\Providers\RouteServiceProvider` class: 

    /**
     * Define your route model bindings, pattern filters, etc.
     *
     * @return void
     */
    public function boot()
    {
        Route::pattern('id', '[0-9]+');
    }

Once the pattern has been defined, it is automatically applied to all routes using that parameter name:

    Route::get('/user/{id}, function ($id) {
        // Executed if {id} is numeric
    });

<a name="named-routes"></a>
## Named Routes

Named routes allow you to generate a URL or redirects for specific routes. It is used by specifying a name for a route by chaining the `name` method onto the route definition, as follows: 

    Route::get('/user/profile', function () {
        //
    })->name('profile');

You may also specify route names for controller actions, as follows:

    Route::get('/user/profile', [
        UserController::class,
        'show'
    ])->name('profile);

> {note} Route names should always be unique.

<a name="generating-urls-to-named-routes"></a>
#### Generating URLs To Named Routes

When you have assigned name to a given route, you may use the route's name when generating URLs or redirects via Lenevor's `route` and `redirect` helper functions, as follows:

    // Generating route
    $url = route('profile');

    // Generating redirects
    return redirect()->route('profile');

If the named route defines parameters, you may pass these parameters as the second argument of the route function. The given parameters will automatically be inserted into the generated URL in their correct positions, as follows: 

    Route::get('/user/{id}/profile', function ($id){
        //
    })->name('profile');

    $url = route('profile', ['id' => 1]);

<a name="route-groups"></a>
## Route Groups

Route groups allow you to share route attributes, such as middleware, prefix, name, namespace, where and domain, across a large number of routes without needing to define those attributes on each individual route. 

Namespace delimiters and slashes in URI prefixes are automatically added where appropriate.

<a name="middleware"></a>
### Middleware

To assign middleware to all routes within a group, you may simply use the `middleware` method before defining the group. Middleware are executed in the order they are listed in the array, as follows: 

    Route::middleware(['first', 'second'])->group(function () {
        Route::get('/', function () {
            // uses first and second middleware
        });

        Route::get('/user', function () {
            // uses first and second middleware
        });
    });

<a name="subdomain-routing"></a>
### Subdomain Routing

Route groups can also be used to manage subdomain routing. Subdomains may be assigned route parameters just like path URIs, allowing you to capture a portion of the domain for usage on your path or controller. The subdomain is specified by calling the `domain` method before defining the group, as follows: 

    Route::domain('{domain}.example.com')->group(function() {
        Route::get('/user/{id}', function ($domain, $id) {
            //
        });
    });

<a name="route-prefixes"></a>
### Route Prefixes

The `prefix` method may be used to prefix each route in the group with a given URI. For example, all route URIs within the group are prefixed with `admin`, as follows:

    Route::prefix('admin')->group(function () {
        Route::get('/users', function () {
            // Result is "/admin/users" URL
        });
    });

<a name="route-name-prefixes"></a>
### Route Name Prefixes

The `name` method may be used to prefix each route name in the group with a given string. For example, you may add the prefix to all grouped route names with `admin`. The prefix of the name of the route is specified and by later it is provided at the end of the route specifies the string of the name of the route. This method must be called before defining the group, as follows: 

    Route::name('admin.')->group(function () {
        Route::get('/users', function () {
            // Route assigned name "admin.users"...
        })->name('users');
    });