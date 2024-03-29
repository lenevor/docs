# HTTP Session

- [Introduction](#introduction)
    - [Configuration](#configuration)
- [Interacting with The Session](#interacting-session)
    - [Retrieving Session Data](#retrieving-session-data)
    - [Storing Data](#storing-data)
    - [Flash Data](#flash-data)


<a name="introduction"></a>
## Introduction

When using applications HTTP requests are stateless, sessions provide a way to store user information across multiple requests. This user information will generally be executed globally with every page load, persistently accessible from subsequent requests.

Lenevor provides a variety of session backends that you access through a simplified API that includes support for backends such as Files.

<a name="configuration"></a>
### Configuration

Your application's session configuration file is stored in `config/session.php`. By default, Lenevor will use the file session driver, which will work for many applications.

The session controller configuration option defines where the session data will be stored for request made by the user. Lenevor uses this controller out of the box:

<div class="content-list" markdown="1">
- `file` - sessions are stored in `storage/framework/sessions`.
- `array` - sessions are stored in a PHP array and will not be persisted.
</div>

> {tip} When a page loads, the session controller will check if the user's browser sends a valid session cookie. If a session cookie does not exist (or if it does not match one stored on the server or has expired), a new session will be created and saved.

<a name="interacting-session"></a>
## Interacting With The Session

Session data is simply an array associated with a particular session ID (cookie).

<a name="retrieving-session-data"></a>
### Retrieving Session Data

Using session data in lenevor is done in two ways: the global session helper and via a Request instance. Let's take a look at session access via a Request instance, which can be type-hinted on a route closure or controller method. As is known of the controller method dependencies are automatically injected via the Lenevor service container:

    <?php

    namespace App\Http\Controllers;

    use App\Http\Controller;
    use Syscodes\Components\Http\Request;

    class HomeController extends Controller
    {
        /**
         * Initialize data for the given user.
         *
         * @param  \Syscodes\Components\Http\Request  $request
         *
         * @return \Syscodes\Components\Http\Response
         */
        public function index(Request $request)
        {
            $value = $request->session()->get('key');

            //
        }
    }

When retrieving an item from the session, you can pass a default value as the second argument to the `get` method. This default value will be returned if the specified key does not exist in the session, therefore the default value of the `get` method will be executed its result returned:

    $value = $request->session()->get('key', 'default');

    $value = $request->session()->get('key', function () {
        return 'default';
    });

<a name="global-session-helper"></a>
#### The Global Session Helper

You may use the global `session` PHP function to retrieve and store data in the session. When the `session` helper is called with a single,  string argument, it will return the value of that session key, also, when the helper is called with a key/value array, those values are stored in the session:

    Route::get('/test', function () {
        // Get item in the session...
        $value = session('key');

        // Specifying a default value...
        $value = session('key', 'default');

        // Store a item of data in the session...
        session(['key' => 'value']);
    });

<a name="retrieving-all-session-data"></a>
#### Retrieving All Session Data

If you would like to retrieve all the data in the session, you may use the all method:

    $value = $request->session()->all();

<a name="determining-item-exists-session">
#### Determining If Item Exists In The Session

To determine if an item is present in the session, you may use the has method. The has method returns true if the item is present and is not null:

    if ($request->session()->has('key)) {
        //
    }

<a name="storing-data"></a>
### Storing Data

For storing data in the session, you will available use the request instance's `put` method or the global `session` helper:

    // Via a request instance...
    $request->session()->put('key', 'value');

    // Via the global "session" helper...
    session(['key' => 'value']);

<a name="push-new-value-session-data"></a>
#### Push New Value To Session Data

The push method is used to push a new value onto a session value that is an array. For example, if the `user.develop` key contains an array of `user.develop`, you can add a new value onto the array like so:

    $request->session()->push('user.developers', 'backend');

<a name="retrieving-deleting-item"></a>
#### Retrieving & Deleting An Item

The `pull` method will retrieve and delete an item from the session in a single statement:

    $request->session()->pull('key', 'default);

<a name="flash-data"></a>
### Flash Data

You may wish to store items in the session for the next request, this is made possible by the `flash` method. Flash data is primarily useful for short-lived status messages:

    $request->session()->flash('status', 'You task was sucessful!');