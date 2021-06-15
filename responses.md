# HTTP Responses

- [Working with Responses](#working-with-responses)
    - [Static Method To Response Render](#static-method-response-render)
    - [Response Objects](#response-objects)
- [Other Response Types](#other-response-types)
    - [View Responses](#view-responses)
    - [JSON Responses](#json-responses)

<a name="working-with-responses"></a>
## Working with Responses

All routes and controllers should return a response to be sent to the user's browser. To do this, Lenevor provides several different ways to return responses. The most typical response is returning a string from a route or controller, which the framework will automatically convert the string into a HTTP response, as follows:

    Route::get('/example', function () {
        return 'This is example...';
    });

You may also return arrays. The framework will automatically convert the array you are working with into a JSON response, as follows: 

    Route::get('/example', function () {
        return [1, 2, 3];
    });

<a name="static-method-response-render"></a>
### Static Method To Response Render

Creates an instance of the same `Response` class for rendering contents to the content, status code and headers using `render` method, as follow:

    use Syscodes\Http\Response;

    Route::get('/example', function () {
        return Response::render('Hello world', 200);
    });

<a name="response-objects"></a>
### Response Objects

It will not only return strings or arrays, also you will be returning full `Syscodes\Http\Response` instances or views.

Returning a full response instance allows you to customize the response's HTTP status code and headers. You can use the `Syscodes\Support\Facades\Response` facade or the `response` helper. Which provides a wide variety of methods for building HTTP responses, as follows: 

    Route::get('/example', function () {
        return response('Hello world', 200)
                        ->header('content-type', 'text-plain');
    });

<a name="other-response-types"></a>
### Other Response Types

The response helper allows you to generate other types of response instances. Therefore, when the response helper is called without arguments, it returns an implementation of the `Syscodes\Contracts\Routing\RouteResponse` [contract](/contracts.md) is returned. This contract provides several helpful methods for generating responses.