# HTTP Requests

- [Introduction](#introduction)
- [Using The Request](#using-request)
    - [Accessing The Request](#accessing-request)
    - [Request Path](#request-path)

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

<a name="using-request-path">
#### Using The Request Path

The `path` method returns the request's path information. For example,  if reception an incoming request is targed at `http://example.com/user/profile`, the `path` method will return `user/profile`, as follows: 

    $path = $request->path();