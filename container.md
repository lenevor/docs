# Service Container

- [Introduction](#introduction)
    - [Inject Service Container In Route](#inject-service-container-route)

<a name="introduction"></a>
## Introduction

The Lenevor service container is a powerful tool that helps organize and instantiate an application's objects, helping to standardize and centralize the way objects are structured in the application to emphasize class dependency management and injection dependency. The service container is characterized be easy , it is very fast and emphasizes an architecture that promotes reusable code. 

Let's look at a simple example:

    <?php

    namespace App\Http\Controllers;

    use App\Http\Controller;
    use Syscodes\Http\Request;

    class UserController extends Controller
    {
        /**
         * The HTTP Request implementation.
         * 
         * @var \Syscodes\Http\Request $request
         */
        protected $request;

        /**
         * Constructor. Create a new Controller instance.
         *
         * @param  \Syscodes\Http\Request  $request
         * 
         * @return void
         */
        public function __construct(Request $request)
        {
            $this->request = $request;
        }

        /**
         * Show the setlocale for given language.
         * 
         * @param  string  $locale
         *
         * @return \Syscodes\Http\Response
         */
        public function show($locale)
        {
            $lang = $this->request->setLocale($locale);

            return view('user.locale', ['lang' => $lang]);
        }
    }

In this example, the `UserController` needs to retrieve data from a language that the user chooses. So, we will inject a service that may recover the language data through `Request`.

Understanding of the Lenevor service container is essential to building a powerful and robust applications, as well as for contributing to Lenevor the core itself. 

<a name="inject-service-container-route"></a>
### Inject Service Container In Route

