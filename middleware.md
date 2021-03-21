# Middleware

- [Introduction](#introduction)
- [Defining Middleware](#defining-middleware)
- [Registering Middleware](#registering-middleware)
    - [Global Middleware](#global-middleware)
    - [Assigning Middleware To Routes](#assigning-middleware-routes)
    - [Middleware Groups](#middleware-groups)
- [Middleware Parameters](#middleware-parameters)

<a name="introduction"></a>
## Introduction

Middleware provide a mechanism inspecting and filtering HTTP requests entering your application. All of middlewares are localted in the `app/Http/Middleware` directory. 

<a name="defining-middleware"></a>
## Defining Middleware

To create a middleware, you must do it (manually for now) from the `app/Http/Middleware` directory, as follows:

    <?php

    namespace App\Http\Middleware;

    use Closure;

    class [Middleware-Name]
    {
        /**
         * Handle an incoming request.
         * 
         * @param  \Syscodes\Http\Request  $request
         * @param  \Closure  $next
         * 
         * @return mixed
         */
        public function handle($request, Closure $next)
        {
            return $next($request);
        }
    }

> {Note} When used [Middleware-Name] in the example, it is for you to change the name of the middleware that you consider necessary for your application. 

<a name="middleware-responses"></a>
#### Middleware & Responses

Actually, a middleware can perform a task before or after passing the request to the application. For example, the following middleware would perform some task before the application uses the request: 

    <?php

    namespace App\Http\Middleware;

    use Closure;

    class [Before-Middleware]
    {
        public function handle($request, Closure $next)
        {
            // Perform action

            return $next($request);
        }
    }

> {Note} When used [Before-Middleware] in the example, it is for you to change the name of the middleware that you consider necessary for your application. 

Therefore, the middleware following would perform its task after the application uses the request: 

    <?php

    namespace App\Http\Middleware;

    use Closure;

    class [After-Middleware]
    {
        public function handle($request, Closure $next)
        {
            $response = $next($request);
            
            // Perform action

            return $response;            
        }
    }

> {Note} When used [After-Middleware] in the example, it is for you to change the name of the middleware that you consider necessary for your application.

<a name="registering-middleware"></a>
## Registering Middleware

<a name="global-middleware"></a>
### Global Middleware

If you want a middleware to run during every HTTP request to your application, you must list the middleware class in the `$middleware` property of your `app/Http/Lenevor.php` class. 