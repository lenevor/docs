# Service Container

- [Introduction](#introduction)

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