# Contracts

- [Introduction](#introduction)
    - [Contracts and Facades](#contracts-and-facades)
- [When To Use Contracts](#when-to-use-contracts)
- [How To Use Contracts](#how-to-use-contracts)
- [Contract Reference](#contract-reference)


<a name="introduction"></a>
## Introduction  

Lenevor's "contracts" are a set of interfaces that define the core services provided by the framework. For example, an `Syscodes\Contracts\Cache\Store` sets functions by the item from the cache store.

All of the Lenevor Contracts haven your own [Github repository](https://github.com/syscodes/contracts).

<a name="contracts-and-facades"></a>
### Contracts And Facades

Lenevor `facades` and helper functions offer an easy way to use Lenevor services without the need to write redundant code and resolve contracts outside of the service container.

Unlike facades, which don't require you to require them in your class constructor, contracts allow you to define explicit dependencies for your classes. 

<a name="when-to-use-contracts"></a>
## When To Use Contracts

The decision to use the contracts or the facades will depend on personal taste or the development team. Both contracts and facades can be used in good quality and solid lenevor applications. For some applications the facades can be used while in others they depend on the contracts. When you know what responsibility your class has, you will notice very few differences between the use of contracts and facades.

In particular, most applications can use facades without complications during development. If you are developing a package that integrates multiple PHP frameworks, you can use the `Syscodes\Components\Contracts` package to integrate Lenevor services and require Lenevor implementations in the `composer.json` file of your package.

<a name="how-to-use-contracts"></a>
## How To Use Contracts

Ok, how do you implement a contract? It's actually quite simple. Most types of classes in Lenevor are resolved through the service container, which include handlers, event listeners, middleware, and even route closures.

For example,  is take a look at this controller: 

    <?php

    namespace App\Http\Controllers;

    use App\Controller\Controller;
    use Syscodes\Components\Contracts\View\Factory;

    class Customer extends Controller
    {
        /**
         * The Request implementation.
         *
         * @var \Syscodes\Components\Contracts\View\Factory $factory
         */
        protected $factory;

        /**
         * Constructor. Create a new Customer instance.
         *
         * @param  \Syscodes\Components\Contracts\View\Factory  $factory;
         *
         * @return void
         */
        publicn function __constructor(Factory $factory)
        {
            $this->factory = $factory;
        }
    }

<a name="contract-reference"></a>
## Contract Reference

This table provides a quick reference to all of the Lenevor contracts and their equivalent facades:

| Contract                                                                                                                       | References Facade            |
|--------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| [Syscodes\Contracts\Cache\Key](https://github.com/syscodes/contracts/blob/{{version}}/Cache/Key.php)                           | &nbsp;                       |
| [Syscodes\Contracts\Cache\Manager](https://github.com/syscodes/contracts/blob/{{version}}/Cache/Manager.php)                   | `Cache`                      |
| [Syscodes\Contracts\Cache\Manager](https://github.com/syscodes/contracts/blob/{{version}}/Cache/Repository.php)                | `Cache::driver()`            |
| [Syscodes\Contracts\Cache\Store](https://github.com/syscodes/contracts/blob/{{version}}/Cache/Store.php)                       | &nbsp;                       |
| [Syscodes\Contracts\Config\Configure](https://github.com/syscodes/contracts/blob/{{version}}/Config/Configure.php)             | `Config`                     |
| [Syscodes\Contracts\Console\Application](https://github.com/syscodes/contracts/blob/{{version}}/Console/Application.php)       | &nbsp;                       |
| [Syscodes\Contracts\Console\Lenevor](https://github.com/syscodes/contracts/blob/{{version}}/Console/Lenevor.php)               | `Prime`                      |
| [Syscodes\Contracts\Container\Container](https://github.com/syscodes/contracts/blob/{{version}}/Container/Container.php)       | `App`                        |
| [Syscodes\Contracts\Cookie\Factory](https://github.com/syscodes/contracts/blob/{{version}}/Cookie/Factory.php)                 | `Cookie`                     |
| [Syscodes\Contracts\Cookie\QueueFactory](https://github.com/syscodes/contracts/blob/{{version}}/Cookie/QueueFactory.php)       | `Cookie::queue()`            |
| [Syscodes\Contracts\Core\Application](https://github.com/syscodes/contracts/blob/{{version}}/Core/Application.php)             | `App`                        |
| [Syscodes\Contracts\Debug\ExceptionHandler](https://github.com/syscodes/contracts/blob/{{version}}/Debug/ExceptionHandler.php) | &nbsp;                       |
| [Syscodes\Contracts\Debug\Handler](https://github.com/syscodes/contracts/blob/{{version}}/Debug/Handler.php)                   | &nbsp;                       |
| [Syscodes\Contracts\Debug\Table](https://github.com/syscodes/contracts/blob/{{version}}/Debug/Table.php)                       | &nbsp;                       |
| [Syscodes\Contracts\Dotenv\Adapter](https://github.com/syscodes/contracts/blob/{{version}}/Dotenv/Adapter.php)                 | &nbsp;                       |
| [Syscodes\Contracts\Dotenv\Repository](https://github.com/syscodes/contracts/blob/{{version}}/Dotenv/Repository.php)           | &nbsp;                       |
| [Syscodes\Contracts\Encryption\Encrypter](https://github.com/Syscodes/contracts/blob/{{version}}/Encryption/Encrypter.php)     | `Crypt`                      |
| [Syscodes\Contracts\Events\Dispatcher](https://github.com/syscodes/contracts/blob/{{version}}/Events/Dispatcher.php)           | `Event`                      |
| [Syscodes\Contracts\Http\Lenevor](https://github.com/syscodes/contracts/blob/{{version}}/Http/Lenevor.php)                     | &nbsp;                       |
| [Syscodes\Contracts\Log\Handler](https://github.com/syscodes/contracts/blob/{{version}}/Log/Handler.php)                       | `Log`                        |
| [Syscodes\Contracts\Pipeline\Pipeline](https://github.com/syscodes/contracts/blob/{{version}}/Pipeline/Pipeline.php)           | &nbsp;                       |
| [Syscodes\Contracts\Routing\Routable](https://github.com/syscodes/contracts/blob/{{version}}/Routing/Routable.php)             | `Route`                      |
| [Syscodes\Contracts\Routing\RouteResponse](https://github.com/syscodes/contracts/blob/{{version}}/Routing/RouteResponse.php)   | `Response`                   |
| [Syscodes\Contracts\Session\Session](https://github.com/syscodes/contracts/blob/{{version}}/Session/Session.php)               | `Session::driver()`          |
| [Syscodes\Contracts\Support\Arrayable](https://github.com/Syscodes/contracts/blob/{{version}}/Support/Arrayable.php)           | &nbsp;                       |
| [Syscodes\Contracts\Support\Deferrable](https://github.com/Syscodes/contracts/blob/{{version}}/Support/Deferrable.php)         | &nbsp;                       |
| [Syscodes\Contracts\Support\Jsonable](https://github.com/Syscodes/contracts/blob/{{version}}/Support/Jsonable.php)             | &nbsp;                       |
| [Syscodes\Contracts\Support\Renderable](https://github.com/Syscodes/contracts/blob/{{version}}/Support/Renderable.php)         | &nbsp;                       |
| [Syscodes\Contracts\Support\Webable](https://github.com/Syscodes/contracts/blob/{{version}}/Support/Webable.php)               | &nbsp;                       |
| [Syscodes\Contracts\View\Engine](https://github.com/syscodes/contracts/blob/{{version}}/View/Engine.php)                       | &nbsp;                       |
| [Syscodes\Contracts\View\Factory](https://github.com/syscodes/contracts/blob/{{version}}/View/Factory.php)                     | `View`                       |
| [Syscodes\Contracts\View\View](https://github.com/syscodes/contracts/blob/{{version}}/View/View.php)                           | `View::make()`               |
| [Syscodes\Contracts\View\ViewFinder](https://github.com/syscodes/contracts/blob/{{version}}/View/ViewFinder.php)               | &nbsp;                       |