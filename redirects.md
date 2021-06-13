# Http Redirects

- [Creating Redirects](#creating-redirects)
- [Redirecting To Named Routes](#redirecting-named-routes)
- [Redirecting To Controller Actions](#redirecting-controller-actions)
- [Redirecting To Responses](#redirecting-responses)
    - [Redirecting The Responses To Home Route](#redirecting-home-route)
    - [Redirecting The Responses To Back Route](#redirecting-back-route)
    - [Redirecting The Responses To Refresh Route](#redirecting-refresh-route)
    - [Redirecting The Responses To Away Route](#redirecting-away-route)
    - [Redirecting The Responses To Secure Route](#redirecting-secure-route)

<a name="creating-redirects"></a>
## Creating Redirects

Redirect response are instances of the `syscodes\Http\RedirectResponse` class that allows to redirect the user to another URL.  To be used this class simplest method is to use the global `redirect` helper:

    Route::get('/dashboard', function () {
        return redirect('/home/dashboard);
    });

<a name="redirecting-named-routes"></a>
## Redirecting To Named Routes

When you call the `redirect` helper with no parameters, an instance of `Syscodes\Routing\Redirector` is returned, allowing any method of this class to be called. An example, generating a `RedirectResponse` to a named route, you can use the `route` method:

    return redirect()->route('home.index');

If your route has parameters, you may pass them as the second argument to the `route` method:

    return redirect()->route('profile', ['id' => 1]);

<a name="redirecting-controller-actions"></a>
## Redirecting To Controller Actions

You may also generate redirects to controller actions. To do so, pass the controller and action name to the `action` method:

    use App\Http\Controllers\ExampleController;

    return redirect()->action(
        [ExampleController::class, 'index']
    );

If your controller route requires parameters, you may pass them as the second argument to the `action` method:

    return redirect()->action(
        [ExampleController::class, 'index'], ['id' => 1]
    );

<a name="redirecting-responses"></a>
## Redirecting To Reponses

Returns redirect of the routes defined by the user. You can redirecting the different routes the methods of `Redirector` class.

<a name="redirecting-home-route"></a>
### Redirecting The Responses To Home Route

Creates a new redirect response to the `home` route:

    return redirect()->home();

<a name="redirecting-back-route"></a>
### Redirecting The Responses To Back Route

Create a new redirect response to the previous location:

    return redirect()->back();

<a name="redirecting-refresh-route"></a>
### Redirecting The Responses To Refresh Route

Create a new redirect response to the current URI:

    return redirect()->refresh();

<a name="redirecting-away-route"></a>
### Redirecting the Responses To Away Route

Create a new redirect response to an external URL (no validation):

    return redirect()->away('https://www.google.com');

<a name="redirecting-secure-route"></a>
### Redirecting The Responses To Secure Route

Create a new redirect response to the given HTTPS path:

    return redirect()->secure('user/profile');