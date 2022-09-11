# Localization

- [Introduction](#introduction)
    - [Configuring The Locale](#configuring-locale)
- [Defining Translation Strings](#defining-translation-strings)
    - [Using Short Keys](#using-short-keys)
    - [Using Translation Strings As Keys](#using-translation-strings-keys)
- [Retrieving Translation Strings](#retrieving-translation-strings)


<a name="introduction"></a>
## Introduction

Lenevor provides a convenient way to retrieve text strings in multiple languages, allowing you to easily support multiple languages within your application.

Lenevor provides a way to manage translation strings. Language strings are stored in files within the `Resources/Lang` directory. Within this directory, there may be directories for each language supported by the application. This is the approach Lenevor uses to translation strings for all of Lenevor's built-in functions, such as file system error messages:

    /resources
        /lang
            /en
                /file.php
            /es
                /file.php

Or, the translation strings may be defined within JSON files where they are located in the `resources/lang` directory. This approach is recommended for application's that have a large number of translatable strings:

    /resources
        /lang
            en.json
            es.json

<a name="configuring-locale"></a>
### Configuring The Locale

The default language for your application is stored in the locale option file `config/app.php`:
      
    'locale' => 'en' 

You can modify the value to suit the needs of your application. Also, you can modify the default language for a single HTTP request at runtime using the `setLocale` method provided by the App facade:

    use Syscodes\Support\Facades\App;

    Route::get ('/example/{locale}', function ($locale) {
        if ( ! in_array ($locale, App::localeSupport ())) {
            abort (400);
        }

        App::setLocale($locale);
    }

Additionally, you can configure a "fallback language", which will be used when the default language does not contain a given translating string. This fallback language is configured in the configuration file `config/app.php`:

    'fallbackLocale' => 'en'

<a name="determining-current-locale"></a>
#### Determining The Current Locale

You may use the currentLocale and isLocale methods on the App facade to determine the current locale or check if the locale is a given value:

    use Syscodes\Support\Facades\App;

    $locale = App::currentLocale();

    if (App::isLocale('en')) {
        //
    }

<a name="defining-translation-strings"></a>
## Defining Translation Strings

<a name="using-short-keys"></a>
### Using Short Keys

Translation strings are stored in files within the `resources/lang` directory. Within this directory, there should be a subdirectory for each language supported by your application. This is how Lenevor uses to manage translation strings for built-in Lenevor features such as exception error messages:

    /resources
        /lang
            /en
                /exception.php
            /es
                /exception.php

All language files return an array of keyed strings. For example:

    <?php

    // resources/lang/en/exception.php

    return [
        'environment' => 'Environment of frame', 
    ];

<a name="using-translation-strings-keys"></a>
### Using Translation Strings As Keys

For applications with a large number of translatable strings, defining each string as a "short key" can become confusing and tedious when referencing those keys in your views. Also, it's cumbersome to constantly invent keys for every translation string supported by your application.

For this reason, Lenevor provides support for defining translation strings using the string's "default" translation as the key. Such translation files that use translation strings as keys are stored in JSON files in the `resources/lang` directory. An example, if your application has a Spanish translation, you should create the `resources/lang/es.json` file:

    {
        "I enjoy programming.": "Disfruto programar."
    }

<a name="retrieving-translation-strings"></a>
## Retrieving Translation Strings

To retrieve translation strings from your language files simply use the `__` helper function. If you are using "short keys" to define your translation strings, you must pass the file containing the key and the key itself to the `__` function using the "dot" syntax. For example, let's retrieve the environment translation string from the `resources/lang/en/exception.php` language file:

    echo __('exception.enviroment');

Again, if the translation string does not exist, the __ function will return the translation string key that it was given.

If you are using the Plaze templating engine, you may use the {{ }} echo syntax to display the translation string:

    {{ __('exception.enviroment') }}

