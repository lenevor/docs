# Locatization

- [Introduction](#introduction)

<a name="instroduction"></a>
## Introduction

Lenevor provides a convenient way to retrieve text strings in multiple languages, allowing you to easily support multiple languages within your application.

Lenevor provides a way to manage translation strings. Language strings are stored in files within the `Resources / Lang` directory. Within this directory, there may be directories for each language supported by the application. This is the approach Lenevor uses to translation strings for all of Lenevor's built-in functions, such as file system error messages:

    /resources
        /lang
            /en
                /file.php
            /es
                /file.php