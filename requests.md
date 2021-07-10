# HTTP Requests

- [Introduction](#introduction)
- [Using The Request](#using-request)
    - [Accessing The Request](#accessing-request)
    - [Request Path](#request-path)
    - [Request Headers](#request-headers)
- [Input](#input)
    - [Retrieving Input](#retrieving-input)

<a name="introduction"></a>
## Introduction

Lenevor's `Syscodes\Http\Request` class is an object-oriented representation of an HTTP request intended to retrieve incoming requests, requests to the application from a browser, outgoing requests, the cookies and files that were submitted with the request.

<a name="using-request"></a>
## Using The Request

<a name="accessing-request"></a>
### Accessing The Request

Is obtained an instance of the HTTP request via dependency injection and is calls the `Syscodes\Http\Request` class on your route closure or controller method. This request instance will automatically be injected by the Lenevor service container, as follows:

    <?php

    namespace App\Http\Controllers;

    use App\Http\Controller;
    use Syscodes\Http\Request;
    
    class ExampleController extends Controller
    {
        /**
         * Get a store.
         *
         * @param  \Syscodes\Http\Request  $request
         *
         * @return \Syscodes\Http\Response
         */
        public function Store(Request $request)
        {
            $url = $request->url();

            //
        }
    }

As specified previous, you may also suggest that the `Syscodes\Http\Request` class used on a route closure. The service container will automatically inject the incoming request into the request when it is executed, as follows:

    use Syscodes\Http\Request;

    Route::get('/', function (Request $request) {
        //
    });

<a name="request-path"></a>
### Request Path

The `Syscodes\Http\Request` class allows a wide variety of methods for examining the incoming and outgoing HTTP request. Below you are exposes the most important methods. 

<a name="using-request-path"></a>
#### Using The Request Path

The `path` method returns the request's path information. For example, if reception an incoming request is targed at `http://example.com/user/profile`, the `path` method will return `user/profile`, as follows: 

    $path = $request->path();

<a name="using-request-decodedPath"></a>
#### Using The Request DecodedPath

The current `decoded path` info for a HTTP request:

    $path = $request->decodedPath();

<a name="using-inspecting-request-path-route"></a>
#### Using Inspecting The Request Path / Route

The `is` method allows you to verify if the incoming request path matches a given pattern. To do this, you may use the `*` character as a wildcard when utilizing this method, as follows:

    if ($request->is('backend/*')) {
        //
    }

Using the `routeIs` method, you may determine if the incoming request has matched a [named route](/routing.md#named-routes):

    if ($request->routeIs('backend.*')) {
        //
    }

<a name="using-request-url"></a>
#### Using The Request URL

If you to use the full URL of the incoming request you may use the `url` method in this case. This `url` method will return the URL without the query string, as follows:

    $path = $request->url();

<a name="using-request-method"></a>
#### Using The Request Method

The `getMethod` method will return the HTTP verb for the request. You may use the `setMethod` method to verify that the HTTP verb matches a given string, as follows:

    $method = $request->getMethod();

    if ($request->setMethod('post')) {
        //
    }

<a name="request-headers"></a>
### Request Headers 

You may retrieve a request header from the `Syscodes\Http\Request` instance using the `header` method. If the header is not present on the request, `null` will be returned. However, the `header` method accepts an optional second argument that will be returned if the header is not present in the request, as follows:

    $header = $request->header('X-Header-Name');

    $header = $request->header('X-Header-Name', 'default');

The `hasHeader` method may be used to determine if the request contains a given header, as follows:

    if ($request->hasHeader('X-Header-Name')) {
        //
    }

<a name="input"></a>
## Input

<a name="retrieving-input"></a>
### Retrieving Input

<a name="retrieving-all-input-data"></a>
#### Retrieving All Input Data

You may retrieve all of the incoming request's input data as an `array` using the `all` method, as follows:

    $rq = $request->all();

<a name="retrieving-input-value"></a>
#### Retrieving An Input Value

You may access all user `input` from the `Syscodes\Http\Request` instance without worrying about which HTTP verb was used for the request, as follows:

    $products = $request->input('products');

You may register a default value as the second argument to the input method. Said value will be returned if the requested input value is not present on the request, as follows: 

    $products = $request->input('products', 'Web Application');

When working with forms that contain array inputs use "dot" notation to access those arrays, as follows: 

    $products = $request->input('products.id);

    $products = $request->input('products.*.name');

You may call the `input` method without any arguments to retrieve all of the input values as an associative array, as follows:

    $rq = $request->input();