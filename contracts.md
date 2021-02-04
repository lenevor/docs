# Contracts

- [Introduction](#introduction)
    - [Contracts and Facades](#contracts-and-facades)
- []

<a name="introduction"></a>
### Introduction

Lenevor's "contracts" are a set of interfaces that define the core services provided by the framework. For example, an `Syscodes\Contracts\Cache\Store` sets functions by the item from the cache store.

All of the Lenevor Contracts haven your own [repository Github](https://github.com/syscodes/contracts).

<a name="contracts-and-facades"></a>
### Contracts And Facades

Lenevor facades and helper functions offer an easy way to use Lenevor services without the need to write redundant code and resolve contracts outside of the service container.

Unlike facades, which don't require you to require them in your class constructor, contracts allow you to define explicit dependencies for your classes. 
