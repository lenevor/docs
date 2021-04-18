# Views

- [Introduction](#introduction)
- [Creating & Rendering Views](#creating-rendering-views)
    - [Nested View Directories](#nested-view-directories)
    - [Determining View Exists](#determining-view-exists)
- [Passing Data To Views](#passing-data-views)
    - [Passing Data With All Views](#passing-data-with-views)

<a name="introduction"></a>
## Introduction

It's not practical to return full strings of HTML documents directly in paths and controllers. Thanks to the fact that Lenevor by means of the views they provide a successful way to place all our HTML in separate files. Views simply separate your controller | application of your presentation logic and are stored in the `resources/views` directory.

A view can be done as follows: 

    <!-- View: resources/views/example.plaze.php -->

    <html>
        <body>
            <h1>Hello, {{ $name }}</h1>
        </body>
    </html>

Since this view is stored in `resources/views/example.plaze.php`, we may return it using the global `view` help as follows:

    Route::get('/', function () {
        return view('example', ['name => 'Alexander]);
    });

or using the "assign" method for detect variables in the views as follows:

    Route::get('/', function () {
        return view('example')->assign('name, 'Alexander);
    });

<a name="creating-rendering-views"></a>
## Creating & Rendering Views

With this utility you may create a view by placing a file with the `.plaze.php` extension in your application in the `resources/views` directory . The `.plaze.php` extension informs the framework that the file contains a [Plaze template](/plaze.md). Plaze templates contain HTML as well as Plaze directives that allow you to easily echo values, create conditional statements (if, elseif), iterate over data, and much more.

Once you have created a view, you can return it from one of your application's routes or controllers using the global view wizard as follows: 

    Route::get('/', function () {
        return view('example', ['name => 'Alexander]);
    });

Views may also return values using the View facade as follows:

    use Syscodes\Support\Facades\View;

    return View::make('example', ['name => 'Alexander]);

As you can see, the first argument passed to the view wizard corresponds to the view file name in the `resources/views` directory. The second argument corresponds is an array of data passing the name variable that is displayed in the view using the [Plaze syntax](/plaze.md). 

<a name="nested-view-directories"></a>
### Nested View Directories

In the views you have the option of being nested within the subdirectories of the `resources/views` directory. "Dot" notation may be used to reference nested views. For example, if your view is stored at `resources/views/backend/profile.plaze.php`, you may return it from one of your application's routes | controllers as follows: 

    return view(backend.profile, $data);

<a name="determining-view-exists"></a>
### Determining If A View Exists

If you are considering determining if a sight exists, you may use the `View` facade. The `exists` method will return `true` if the view exists as follows:

    use Syscodes\Support\Facades\View;

    if (View::exists('users.name)) {
        //
    }

<a name="passing-data-views"></a>
## Passing Data To Views

As you realized in the previous examples, you can pass an array of data to the view to make that data available to the view:

    return view('example', ['name => 'Alexander]);

When passing information in this manner, the data should be an array with key / value pairs. After providing data to a view, you can then access each value within your view using the data's keys, such as `<?php echo $name; ?>`.

As an alternative to passing a complete array of data to the `view` helper function, you may use the `assign` method to add individual pieces of data to the view. The `assign` method returns an instance of the view object so that you can continue chaining methods before returning the view as follows:

    return view('example')
           ->assign('name', 'Alexander')
           ->assign('lastname', 'Campo');

<a name="passing-data-with-views"></a>
### Passing Data With All Views

Sometimes, you may need to share data with all the views that are rendered  by your application. You may do it using the `View` facade's the `share` method. Generally, you should place calls to the shared method within a service provider's `boot` method . You can add them to the `App\Providers\AppServiceProvider` class or generate a separate service provider to house as follows:

    <?php 

    namespace App\Providers;

    use Syscodes\Support\Facades\View;
    use Syscodes\Support\ServiceProvider;

    class AppServiceProvider extends ServiceProvider
    {
        /**
        * Bootstrap any application services.
        * 
        * @return void
        */
        public function boot()
        {
            View::share('key', 'value');
        }

        /**
        * Register any application services.
        * 
        * @return void
        */
        public function register()
        {
            //
        }
    }