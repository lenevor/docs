# Plaze Templates

- [Introduction](#introduction)
- [Displaying Data](#displaying-data)
- [Plaze Directive](#plaze-directive)
    - [If Directive](#if-directive)
    - [Switch Directive](#switch-directive)
    - [Comments](#comments)
    - [Including Subviews](#including-subviews)
    - [Raw PHP](#raw-php)
- [Building Layouts](#building-layouts)
    - [Defining To Layouts Template Inheritance](#defining-layouts-template-inheritance)
    - [Defining To Layouts Components](#defining-layouts-components)
- [Debugging Directive](#debugging-directive)
- [Forms](#forms)
    - [CSRF Field](#csrf-field)
    - [Method Field](#method-field)

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

<a name="rendering_json"></a>
#### Rendering JSON

Also, you have the option of passing an array to your view in order to represent in JSON notation to initialize a Javascript variable. For example: 

    var json = <?php echo json_encode($array) ?>

However, instead of manually calling `json_encode`, you may use the `@json` Plaze directive. The `@json` directive accepts the same arguments as PHP's `json_encode` function. By default, the @json directive calls the json_encode function with the `JSON_HEX_TAG`, `JSON_HEX_APOS`, `JSON_HEX_AMP`, and `JSON_HEX_QUOT` flags:

    <script>

        var json = @json($array)

        var json = @json($array, JSON_PRETTY_PRINT)

    </script>

<a name="displaying-unescaped-data"></a>
#### Displaying Unescaped Data

By default, Plaze `{{}}` statements are automatically sent  through PHP's `htmlspecialchars` function to prevent XSS attacks. If you do not want your data to be escaped, you may use the following syntax of like this: 

    What's your name? My name is {!! $name !!}.

<a name="literal-directive"></a>
#### The `<@Literal` Directive

If you want, you can wrap the HTML in the `<@literal` directive so you don't have to prefix each `Plaze` echo statement with an `@` symbol, follows: 

    <@literal
        <div class="example">
            What's your name? My name is {{ $name }}.
        </div>
    <@endliteral

<a name="plaze-directive"></a>
## Plaze Directive

The `Plaze` templates also provide access to common PHP control structures, such as conditional statements and loops. In this way, they provide a very clean, terse way of working with PHP control structures, and at the same time, also remaining similar to their PHP counterparts.

<a name="if-statements"></a>
### If Statements

You may construct basic conditional statements to handle `if` syntax using the directives `<@if`, `<@elseif`, `<@else`. All `if` blocks must be closed with the directive `<@endif`. These directives function identically to their PHP counterparts, as follows:

    <@if ($name == 'Alexander')
        <h1>Welcome, Alexander</h1>
    <@elseif ($name == 'guest')
        <h1>Welcome, guest user</h1>
    <@else 
        <h1>Not found user</h1>
    <@endif

The `<@isset` and `<@empty` directives can be used as replacements for their respective PHP functions, as follows:

    <@isset ($name)
        // If $name is defined
    <@endisset

    <@empty ($name)
        // If $name is empty
    <@endempty

<a name="switch-statements"></a>
### Switch Statements

Switch statements can be constructed using the `<@switch`, `<@case`, `<@break` and `@default` directives . The block must be closed with `<@endswitch`, as follows:

    <@switch($i)
        <@case(1)
            First case...
            <@break
            
        <@case(2)
            Second case...
            <@break
            
        <@default
            Default case...
    <@endswitch

<a name="loops-statements"></a>
### Loop Statements

In the conditional statements, Plaze provides simple directives for working with PHP's loop structures. Again, each of these directives functions identically to their PHP counterparts, as follows:

    <@for ($i = 0; $i < 10; $i++)
        The value is {{ $i }}
    <@endfor

    <@foreach ($users as $user)
        <p>This is user {{ $user->id }}</p>
    <@endforeach

    <@forelse ($users as $user)
        <li>{{ $user->name }}</li>
    <@empty
        <p>No users</p>
    <@endforelse

    <@while (true)
        <p>I'm looping forever.</p>
    <@endwhile

When using loops you may also end the loop or skip the current iteration using the `<@continue` and `<@break` directives, as follows:

    <@foreach ($users as $user)
        <@if ($user->status == 1)
            <@continue
        <@endif
    
    <li>{{ $user->name }}</li>

        <@if ($user->count == 10)
            <@break
        <@endif
    <@endforeach

You may also include parameters in the continuation or break condition, as follows:

    <@foreach ($users as $user)
        <@continue ($user->status == 1)
    
    <li>{{ $user->name }}</li>

        <@break ($user->count == 10)
    <@endforeach

<a name="comments"></a>
### Comments

Plaze also allows you to define comments in your views. However, unlike HTML comments, Plaze comments are not included in the HTML returned by your application, as follows:

    {{-- This is a comment not be present in the rendered HTML --}}

<a name="including-subviews"></a>
### Including Subviews

Plaze's `@include` directive allows you to include a Plaze view from within another view. All variables that are available to the parent view will be made available to the included view, as follows:

    <div>
        <@include ('partial.menu')
    </div>

Even though the included view will inherit all data available in the parent view, you may also pass an array of additional data that should be made available to the included view, as follows:

    <@include ('partial.menu', ['name' => 'about'])

<a name="raw-php"></a>
### Raw PHP

In some situations, it's useful to embed PHP code into your views. You can use the PHP `<@php` directive to execute a block of plain PHP within your template, as follows:

    <@php
        phpinfo()
    <@endphp

<a name="building-layouts"></a>
## Building Layouts

<a name="defining-layouts-template-inheritance"></a>
### Defining To Layouts Template Inheritance

Of the primary benefits of using Plaze are template inheritance and sections. First, we will examine a "master" page layout. Since most web applications maintain the same general layout on various pages, therefore, it's convenient to define this layout in a single Plaze view, as follows:

    {{-- Path: resources/layouts/app.plaze.php --}}

    <!Doctype html>
    <html>
        <head>
            <title>Project - <@give('title')</title>
        </head>
        <body>
            <@section('sidebar')
                This is the sidebar
            <@show

            <div class="content">
                <@give('content)
            </div>
        </body>
    </html>

Now as you can see, this file contains a typical HTML markup. However, be aware of the `<@section` and` <@give` directives. The `<@section` directive defines a section of content, while the` <@give` directive is used to display the content of a certain section.

We have already defined a layout for our application, let's define a child page that inherits the layout. 

<a name="extending-layout"></a>
#### Extending A Layout

When defining a child view, use the `<@extends` Plaze directive to specify which layout should" inherit "that child view. Views that extend a Plaze layout can inject content into sections using the `<@section` directive. Remember, as in the previous example, the contents of these sections will be displayed in the layout using the `<@give` directive, as follows:

    {{-- Path: resources/views/welcome.plaze.php --}}

    <@extends('layouts.app')

    <@section('title', 'EXample 1')

    <@section('sidebar')
        <@parent

        <p>This is appended to the master sidebar</p>
    <@stop

    <@section('content')
        <p>This is content body</p>
    <@stop

In the example above, the sidebar section is utilizing the `<@parent` directive to append (rather than overwriting) content to the layout's sidebar. This directive will be replaced by the content of the layout when the view is rendered.

>{tip} Contrary to the previous example, this sidebar section ends with `<@endsection` instead of `<@show`. The `<@endsection` directive will only define a section while `<@show` will define and immediately give the section.

The `<@give` directive also accepts a default value as the second parameter. This value is shown if the section being yielded is not defined in the inherited design, as follows:

    <@give('content', 'Default content')

<a name="defining-layouts-components"></a>
#### Defining To Layouts Components

`Components` and `slots` allow benefits similar to `sections` and `layouts`. `Components` and `slots` allow similar benefits to sections and layouts. However, the `components` and the `slots` have the particularity of being reused in any part of one or in various web applications: 

    {{-- Path: resources/views/alert.plaze.php --}}

    <div class="alert alert-danger">
        {{ $slot }}
    </div>

The `{{ $slot }}` variable will contain the content we wish to inject into the component. Now, to construct this component, we can use the `<@component` Plaze directive, as follows:

    <@component('alert')
        <strong>Whoops!</strong> Something went wrong!
    <@endcomponent

Sometimes it is very useful to define multiple slots for a component. Now, we are going to modify our alert component to allow the injection of a "title", as follows: 

    {{-- Path: resources/views/alert.plaze.php --}}

    <div class="alert alert-danger">
        <div class="alert-title">{{ $title }}</div>

        {{ $slot }}
    </div>

Now, we can inject content into the named slot using the `<@slot` directive. Any content not within a `<@slot` directive will be passed to the component in the `$slot` variable, as follows:

    <@component('alert')
        <@slot('title')
        
            Forbidden
        <@endslot
        
        You are not allowed to access this resource!
    <@endcomponent

<a name="debugging-directive"></a>
## Debugging Directive

With the directive `<@dd` you can return results of variables, arrays and direct APIs in the HTML, in order to escape correct values according to the query that is being carried out, as follows: 

    <pre>
        <@dd($result)
    </pre>

<a name="forms"></a>
## Forms

<a name="csrf-field"></a>
### CSRF Field 

Anytime you define an HTML form in your application, you should include a hidden CSRF token field in the form so that the CSRF protection middleware can validate the request. You may use the `<@csrf` Plaze directive to generate the token field, as follows:

    <form action="/user" method="POST">
        <@csrf

        ...
    </form>

<a name="method-field"></a>
### Method Field

When trying to use the `PUT`, 'PATCH' or 'DELETE' requests they cannot be used in HTML forms directly, to carry out these requests you will need to add a hidden` _method` field to spoof these HTML verbs. The `<@ method` plaze directive can create this field for you, as follows: 

    <form action="/user/guest" method="POST">
        <@method('PUT')

        ...
    </form>