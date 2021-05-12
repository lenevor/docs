# Plaze Templates

- [Introduction](#introduction)

<a name="introduction"></a>
## Introduction

Plaze is the simple, and very powerful template engine that is included with Lenevor. Unlike other PHP template engines, Plaze does not restrict you from using simple PHP code in your templates. In fact, all Plaze templates are transpiled into plain PHP code and cached until a modification is notified, this meaning that Plaze adds zero overhead to your application. Plaze template files use the `.plaze.php` file extension and are typically stored in the 'resources/views' directory.

Plaze template views can be returned from routes or controller using the global `view` helper. Data may be passed to the Plaze view using the `view` helper's second argument: 

    Route::get('/', function () {
        return view('welcome', ['name => 'Alexander']);
    });

