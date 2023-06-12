# Localization

- [Introduction](#introduction)
    - [Configuring The Locale](#configuring-locale)
- [Defining Translation Strings](#defining-translation-strings)
    - [Using Short Keys](#using-short-keys)
    - [Using Translation Strings As Keys](#using-translation-strings-keys)
- [Retrieving Translation Strings](#retrieving-translation-strings)
    - [Replacing Parameters In Translation Strings](#replacing-parameters-translation-strings)
    - [Pluralization](#pluralization)


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

If you are using your default translation strings as your translation keys, you should pass the default translation of your string to the __ function:

    echo __('I enjoy programming.');

Again, if the translation string does not exist, the __ function will return the translation string key that it was given.

If you are using the [Plaze templating engine](/plaze.md), you may use the {{ }} echo syntax to display the translation string:

    {{ __('exception.enviroment') }}

<a name="replacing-parameters-translation-strings"></a>
### Replacing Parameters In Translation Strings

If you wish, you may use placeholders in your translation strings. All placeholders are prefixed with a `:`. For example, you may define a welcome message with a placeholder name, as follows:

    'welcome' => 'Welcome, :name',

To replace the placeholders when retrieving a translation string, you may use an array as the second argument to the `__` function, as follows:

    {{ __('views.welcome', ['name' => 'Alexander']) }}


If your placeholder has all capital letters, or has only its first letter capitalized, the translated value will be capitalized accordingly, as follows:

    'welcome' => 'Welcome, :NAME', // Welcome, ALEXANDER
    'Hello'   => 'Hello, :Name',   // Hello, Elena

<a name="pluralization"></a>
### Pluralization

You may pass an array of values to replace placeholders in the traduction string as the second parameter to the `__` function. This allows for very simple number translations and formatting, as follow:

    [Setting file]
    'fruit-apples' => 'I have {0, number} apples',

    [Plaze file]
    {{ __('view.fruit-apples', [3]) }} // I have 3 apples

The first item in the placeholder corresponds to the index of the item in the array, if it’s numerical, as follow:

    [Setting file]
    'person' => 'The top {1, number} men out-performed the remaining {0, number}',

    [Plaze file]
    {{ __('view.person', [20, 30]) }} // The top 30 men out-performed the remaining 20

You can also use named keys to make it easier to keep things straight, if you’d like, as follow:

    [Setting file]
    'fruits-oranges' => 'I have {number_oranges, number, integer} oranges',

    [Plaze file]
    {{ __('view.fruits-oranges', ['number_oranges' => 3]) }} // I have 3 oranges

Also, you may do more than just number replacement. According to the [official ICU docs](https://unicode-org.github.io/icu-docs/apidoc/released/icu4c/classMessageFormat.html#details) for the underlying library, the following types of data can be replaced:

- numbers - integer, currency, percent
- dates - short, medium, long, full
- time - short, medium, long, full
- spellout - spells out numbers (i.e., 34 becomes thirty-four)
- ordinal
- duration

Here are a few examples in the setting file of language:
    
    'shortTime'  => 'The time is now {0, time, short}',
    'mediumTime' => 'The time is now {0, time, medium}',
    'longTime'   => 'The time is now {0, time, long}',
    'fullTime'   => 'The time is now {0, time, full}',
    'shortDate'  => 'The date is now {0, date, short}',
    'mediumDate' => 'The date is now {0, date, medium}',
    'longDate'   => 'The date is now {0, date, long}',
    'fullDate'   => 'The date is now {0, date, full}',
    'spelledOut' => '34 is {0, spellout}',
    'ordinal'    => 'The ordinal is {0, ordinal}',
    'duration'   => 'It has been {0, duration}',

This is the display of the translation string in the Plaze file in a view, using to the `__` function for to show the message already translated, as follow:

    {{ __('view.shortTime', [time()]) }} // "The time is now 09:10 PM"
    {{ __('view.mediumTime', [time()]) }} // "The time is now 09:15:50 PM"
    {{ __('view.longTime', [time()]) }} // "The time is now 09:19:09 PM CDT"
    {{ __('view.fullTime', [time()]) }} // "The time is now 09:20:26 PM Coordinated Universal Time"
    {{ __('view.shortDate', [time()]) }} // "The date is now 11/18/21"
    {{ __('view.mediumDate', [time()]) }} // "The date is now Nov 18, 2021"
    {{ __('view.longDate', [time()]) }} // "The date is now November 18, 2021"
    {{ __('view.fullDate', [time()]) }} // "The date is now Sunday, November 21, 2021"
    {{ __('view.spelledOut', [34]) }} // "34 is thirty-four"
    {{ __('view.ordinal', [time()]) }} // "It has been 1,686,592,572nd"
    {{ __('view.duration', [time()]) }} // "It has been 468,453:21:18"