# HTTP Responses

- [Working with Responses](#working-with-responses)

<a name="working-with-responses">
## Working with Responses

All routes and controllers should return a response to be sent to the user's browser. To do this, Lenevor provides several different ways to return responses. The most typical response is returning a string from a route or controller, which the framework will automatically convert the string into a HTTP response, as follows:

    Route::get('/example', function () {
        return 'This is example...';
    });

You may also return arrays. The framework will automatically convert the array you are working with into a JSON response, as follows: 

    Route::get('/example', function () {
        return [1, 2, 3];
    });

