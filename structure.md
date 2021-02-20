# Directory Structure

- [Introduction](#introduction)
- [The Root Directory](#the-root-directory)
    - [The `app` Directory](#the-root-app-directory)
    - [The `bootstrap` Directory](#the-bootstrap-directory)
    - [The `config` Directory](#the-config-directory)
    - [The `database` Directory](#the-database-directory)
    - [The `public` Directory](#the-public-directory)
    - [The `resources` Directory](#the-resources-directory)
    - [The `routes` Directory](#the-routes-directory)
    - [The `storage` Directory](#the-storage-directory)
    - [The `vendor` Directory](#the-vendor-directory)
- [The App Directory](the-app-directory)
    - [The `Console` Directory](#the-console-directory)
    - [The `Events` Directory](#the-events-directory)
    - [The `Exceptions` Directory](#the-exceptions-directory)
    - [The `Http` Directory](#the-http-directory)
    - [The `Listeners` Directory](#the-listeners-directory)
    - [The `Models` Directory](#the-models-directory)
    - [The `Policies` Directory](#the-policies-directory)
    - [The `Providers` Directory](#the-providers-directory)

<a name="introduction"></a>
## Introduction

The default Lenevor application structure  is intended to provide excellent performance in creating applications large and small. However, you are free to organize your application however you want convenient. Lenevor does not impose restrictions on where a certain class is located, as long as it is registered from the `bootstrap/register/autoloadClassmap.php` file or from Composer it can automatically load the class.

<a name="the-root-directory">
## The Root Directory

<a name="the-app-directory">
## The App Directory