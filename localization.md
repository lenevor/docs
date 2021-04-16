# Localization

- [Introduction](#introduction)
    - [Configuring The Locale](#configuring-locale)

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

