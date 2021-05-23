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