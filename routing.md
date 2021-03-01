# Routing

- [Basic Routing](#basic-routing)
    - [Redirect Routes](#redirect-routes)
    - [View Routes](#view-routes)
- [Route Parameters](#route-parameters)
    - [Required Parameters]($required-parameters)
    - [Regular Expression Constraints](#regular-expression-constraints)
    - [Global Constraints](#global-constraints)
    
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

Sometimes you will need to capture segments of the URI within your route. For example, you may need to capture a product's ID form the URL. You may do so by defining route parameters:

    Route::get('/product/{id}, function ($id) {
        return 'Product ID: '.$id;
    });
