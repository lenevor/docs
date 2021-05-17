# Plaze Templates

- [Introduction](#introduction)
- [Displaying Data](#displaying-data)

<a name="introduction"></a>
## Introduction

Plaze is the simple, and very powerful template engine that is included with Lenevor. Unlike other PHP template engines, Plaze does not restrict you from using simple PHP code in your templates. In fact, all Plaze templates are transpiled into plain PHP code and cached until a modification is notified, this meaning that Plaze adds zero overhead to your application. Plaze template files use the `.plaze.php` file extension and are typically stored in the 'resources/views' directory.

Plaze template views can be returned from routes or controller using the global `view` helper. Data may be passed to the Plaze view using the `view` helper's second argument: 

    Route::get('/', function () {
        return view('welcome', ['name' => 'Alexander']);
    });

<a name="displaying-data"></a>
## Displaying Data

You may display the data that is passed to your Plaze views, you simply specifying the variable in braces. For example,  the following path:

    Route::get('/', function () {
        return view('welcome', ['name'  => 'Alexander']);
    });

You may display the contents of the name variable, as follows:

    Hello, {{ $name }}

> {tip} Plaze's {{ }} echo statements are automatically sent through PHP's htmlspecialchars function to prevent XSS attacks.