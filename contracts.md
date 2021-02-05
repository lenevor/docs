# Contracts

- [Introduction](#introduction)
    - [Contracts and Facades](#contracts-and-facades)
- [When To Use Contracts](#when-to-use-contracts)
- [How To Use Contracts](#how-to-use-contracts)

<a name="introduction"></a>
### Introduction

Lenevor's "contracts" are a set of interfaces that define the core services provided by the framework. For example, an `Syscodes\Contracts\Cache\Store` sets functions by the item from the cache store.

All of the Lenevor Contracts haven your own [Github repository](https://github.com/syscodes/contracts).

<a name="contracts-and-facades"></a>
### Contracts And Facades

Lenevor facades and helper functions offer an easy way to use Lenevor services without the need to write redundant code and resolve contracts outside of the service container.

Unlike facades, which don't require you to require them in your class constructor, contracts allow you to define explicit dependencies for your classes. 

<a name="when-to-use-contracts"></a>
## When To Use Contracts

The decision to use the contracts or the facades will depend on personal taste or the development team. Both contracts and facades can be used in good quality and solid lenevor applications. For some applications the facades can be used while in others they depend on the contracts. When you know what responsibility your class has, you will notice very few differences between the use of contracts and facades.

In particular, most applications can use facades without complications during development. If you are developing a package that integrates multiple PHP frameworks, you can use the `Syscodes\Contracts` package to integrate Lenevor services and require Lenevor implementations in the `composer.json` file of your package.

<a name="how-to-use-contracts"></a>
## How To Use Contracts

Ok, how do you implement a contract? It's actually quite simple. Most types of classes in Lenevor are resolved through the service container, which include handlers, event listeners, middleware, and even route closures.

For example,  is take a look at this controller: 

    <?php

    namespace App\Http\Controllers;

    use Syscodes\Contracts\View\Factory;

    class Customer
    {
        /**
         * The Request implementation.
         *
         * @var \Syscodes\Contracts\View\Factory $factory
         */
        protected $factory;

        /**
         * Constructor. Create a new Customer instance.
         *
         * @param  \Syscodes\Contracts\View\Factory  $factory;
         *
         * @return void
         */
        publicn function __constructor(Factory $factory)
        {
            $this->factory = $factory;
        }
    }