# Installation

- [Meet Lenevor](#meet-lenevor)
    - [Why Lenevor?](#why-lenevor)
- [Your First Lenevor Project](#your-first-lenevor-project)
    - [Installation Via Composer](#installation-via-composer)
- [Initial Configuration](#initial-configuration)

<a name="meet-lenevor"></a>
## Meet lenevor

Lenevor is a web application framework with light and simple syntax. Our foundation is that web development should be a pleasant and creative experience for so truly fulfilling. Lenevor it's created as an alternative web development by easing common tasks used in many web projects.

Lenevor it strives to stay simple, by focusing on the basics, such as:

- A simple, fast routing engine.
- Powerful dependency injection container.
- Connection with the database using Query Builder.
- Multiple back-ends for session and cache storage.
- Real-time event broadcasting.
- Separation between the bussines logic and presentation through the Plaze template engine.
- Lenevor is accessible, powerful, and provides tools required for large, robust applications.

Whether you are new to PHP or web frameworks or have years of experience, Lenevor is a framework that can grow with you. We'll help you take your first steps as a web developer or give you a boost as you take your expertise to the next level. We can't wait to see what you build.

<a name="why-lenevor"></a>
### Why lenevor?

There are a variety of tools and frameworks available to you when building a web application. However, we believe Lenevor is the best choice for building modern, full-stack web applications.

#### A Progressive Framework

We like to call Lenevor a "progressive" framework. By that, we mean that Lenevor grows with you. If you're just taking your first steps into web development, Lenevor's library of documentation and guides, will help you learn the ropes without becoming overwhelmed. However, it is under development.

#### A Scalable Framework

Lenevor is incredibly scalable. Thanks to the scaling-friendly nature of PHP and lenevor's built-in support for fast, distributed cache systems like Redis, horizontal scaling with lenevor is a breeze. In fact, Lenevor applications have been easily scaled to handle hundreds of millions of requests per month.

<a name="your-first-lenevor-project"></a>
## Your First lenevor Project

We want it to be as easy as possible to get started with Lenevor. There are a variety of options for developing and running a Lenevor project on your own computer. 

<a name="installation-via-composer"></a>
### Installation Via Composer

If your computer already has PHP and Composer installed, you may create a new Lenevor project by using Composer directly. After the application has been created, you may start [PHP, Wamp, nginx, etc.] local development server using the navigator web:

    composer create-project lenevor/lenevor [project-app]

    cd [project-app]

    php prime

<a name="initial-configuration"></a>
## Initial Configuration

All of the configuration files for the Lenevor framework are stored in the `config` directory. Each option is documented, so feel free to look through the files and get familiar with the options available to you.

Lenevor needs almost no additional configuration out of the box. You are free to get started developing! However, you may wish to review the `config/app.php` file and its documentation.

<a name="environment-configuration"></a>
#### Environment Based Configuration

Since many of Lenevor's configuration option values may vary depending on whether your application is running on your local computer or on a production web server, many important configuration values are defined using the `.env` file that exists at the root of your application.

Your `.env` file should not be committed to your application's source control, since each developer / server using your application could require a different environment configuration. Furthermore, this would be a security risk in the event an intruder gains access to your source control repository, since any sensitive credentials would get exposed.