# HTTP Responses

- [Working with Responses](#working-with-responses)
    - [Static Method To Response Render](#static-method-response-render)
    - [Response Objects](#response-objects)
- [Other Response Types](#other-response-types)
    - [View Responses](#view-responses)
    - [JSON Responses](#json-responses)
    - [No Content To Responses](#no-content-responses)

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
                    ->header('content-type', 'text/plain');
    });

<a name="other-response-types"></a>
## Other Response Types

The response helper allows you to generate other types of response instances. Therefore, when the response helper is called without arguments, it returns an implementation of the `Syscodes\Contracts\Routing\RouteResponse` [contract](/contracts.md) is returned. This contract provides several helpful methods for generating responses.

<a name="view-responses"></a>
### View Responses

If you need control over the response's status and headers but also need to return a [view](/views.md) as the response's content, you should use the `view` method, as follows: 

    return response()
                ->view('welcome', $data, 200)
                ->header('content-type', $type);

<a name="json-responses"></a>
### JSON Responses

The `json` method will automatically set the `Content-Type` header to `application/json`, as well as convert the given array to JSON using the `json_encode` PHP function, as follow:

    return response()->json([
            'name' => 'Alexander'
            'Occupation' => 'Full-Stack Developer'
        ]);

<a name="no-content-responses"></a>
### No Content To Responses

If you need not to return content from the response's but instead controlling the response's status and header  you may do it with the `noContent` method, but also may implement the setContent method of the `Response` class to return a view, as follows:

    return response()
                ->noContent(200, ['content-type' => 'text/plain'])
                ->setContent('welcome');

Also adding the `view` helper, as follows:

    return response()
                ->noContent(200, ['content-type' => 'text/html'])
                ->setContent(view('welcome'));