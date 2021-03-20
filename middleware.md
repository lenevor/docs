# Middleware

- [Introduction](#introduction)
- [Defining Middleware](#defining-middleware)
- [Registering Middleware](#registering-middleware)
    - [Global Middleware](#global-middleware)
    - [Assigning Middleware To Routes](#assigning-middleware-routes)
    - [Middleware Groups](#middleware-groups)
- [Middleware Parameters](#middleware-parameters)
- [Terminable Middleware](#terminabe-middleware)

<a name="introduction"></a>
## Introduction

Middleware provide a mechanism inspecting and filtering  HTTP requests entering your application. All of middlewares are localted in the `app/Http/Middlware` directory. 

<a name="defining-middleware">
## Defining Middlware

To create a middleware, you must do it (manually for now) from the `app/Http/Middleware` directory, as follows:

    <?php

    namespace App\Http\Middleware;

    use Closure;

    class [middleware-name]
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

