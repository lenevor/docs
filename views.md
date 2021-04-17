# Views

- [Introduction](#introduction)
- [Creating & Rendering Views](#creating-rendering-views)

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

Since this view is stored in `resources/views/example.plaze.php`, we may return it using the global view help as follows:

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
