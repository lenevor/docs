# Database

- [Introduction](#introduction)
    - [Configuration](#configuration)
- [Basic Usage SQL Queries](#basic-usage-sql-queries)
    - [Using Multiple Database Connections](#using-multiple-database-connections)

<a name="introduction"></a>
## Introduction

Most modern web applications interact with a database. Lenevor makes interacting with databases simple across a variety of supported databases using raw SQL, a `fluent query builder`, and the `Erostrine ORM`. Currently, Lenevor provides support for four databases:

- MySQL 5.7+ 
- PostgreSQL 9.6+
- SQLite 3.8.8+
- SQL Server 2017+

<a name="configuration"></a>
### Configuration

For the configuration of Lenevor's database services, it must be accessed in the `config/database.php` file of your application. In this file, you may define all of your database connections, as well as which connection should be used by default. Most of the configuration in this file are controlled by your application's environment variable registers. However, you are free to modify your database configuration as needed for your local database.

<a name="mysql-configuration"></a>
#### MySQL Configuration

MySQL database is defined from the environment variables file of your application's, as follows:

    DB_CONNECTION = mysql
    DB_HOST = 127.0.0.1
    DB_PORT = 3306
    DB_DATABASE = lenevor
    DB_USERNAME = home
    DB_PASSWORD = hidden

<a name="postgresql-configuration"></a>
#### PostgreSQL Configuration

PostgreSQL database is defined from the environment variables file of your application's, as follows:

    DB_CONNECTION = pgsql
    DB_HOST = 127.0.0.1
    DB_PORT = 5402
    DB_DATABASE = lenevor
    DB_USERNAME = home
    DB_PASSWORD = hidden

<a name="sqlite-configuration"></a>
#### SQLite Configuration

SQLite databases are contained in a single file on your filesystem specifically in `database/data/database.sqlite`. After the database has been created, you may easily configure your environment variables to point to this database by recording the absolute path to the database in the `DB_DATABASE` environment variable, as follows:

    DB_CONNECTION = sqlite
    DB_DATABASE = /absolute/path/to/database.sqlite

<a name="microsoft-sql-server-configuration"></a>
#### Microsoft SQL Server Configuration

To use a Microsoft SQL Server database, you should ensure that you have the sqlsrv and pdo_sqlsrv PHP extensions installed as well as any dependencies they may require such as the Microsoft SQL ODBC driver. Also, is defined your configure from the environment variables file of your application's, as follows:

    DB_CONNECTION = sqlsrv
    DB_HOST = 127.0.0.1
    DB_PORT = 1433
    DB_DATABASE = lenevor
    DB_USERNAME = home
    DB_PASSWORD = hidden

<a name="basic-usage-sql-queries"></a>
## Basic Usage SQL Queries

When you have configured your database connection, you may run queries using the `DB facade`. This facade provides methods for each type of query: `select`, `update`, `insert`, `delete` and `statement`.

<a name="usage-select-query"></a>
#### Usage A Select Query

To run a basic `SELECT` query, you may use the `select` method on the DB facade:

    <?php
    
    namespace App\Http\Controllers;
    
    use App\Http\Controllers\Controller;
    use Syscodes\Components\Support\Facades\DB;
    
    class UserController extends Controller
    {
        /**
         * Show a list of all of the application's users.
         * 
         * @return \Syscodes\Components\Http\Response
         */
        public function index()
        {
            $users = DB::select('select * from users where status = ?', [1]);
            
            return view('user.index', ['users' => $users]);
        }
    }

The select method will always return an array of results. Each result within the array will be a PHP stdClass object representing a record from the database:

    use Syscodes\Components\Support\Facades\DB;
    
    $users = DB::select('select * from users');
    
    foreach ($users as $user) {
        echo $user->name;
    }

<a name="using-named-bindings"></a>
#### Using Named Bindings

Instead of using `?` to represent your parameter bindings, you may execute a query using named bindings:

    $sql = DB::select('select * from users where user_id = :id', ['id' => 1]);

<a name="running-insert-statement"></a>
#### Running An Insert Statement

To execute an `insert` statement, you may use the insert method on the DB facade. Like select, this method accepts the SQL query as its first argument and bindings as its second argument:

    use Syscodes\Components\Support\Facades\DB;

    DB::insert('insert into users (id, name) values (?, ?)', [1, 'Marc']);

<a name="running-update-statement"></a>
#### Running An Update Statement

The `update` method should be used to update existing records in the database. The number of rows affected by the statement is returned by the method:

    use Syscodes\Components\Support\Facades\DB;

    $sql = DB::update(
        'update users set age = 35 where name = ?',
        ['Elena']
    );

<a name="running-delete-statement"></a>
#### Running A Delete Statement

The `delete` method should be used to delete records from the database. Like update, the number of rows affected will be returned by the method:

    use Syscodes\Components\Support\Facades\DB;

    $sql = DB::delete('delete from users');

<a name="runing-general-statement"></a>
#### Running A General Statement

Some database statements do not return any value. For these types of operations, you may use the `statement` method on the DB facade:

    DB::statement('drop table users');

<a name="using-multiple-database-connections"></a>
### Using Multiple Database Connections

You may defiYou may define multiple connections in your `config/database.php` configuration file, through the connection method provided by the DB facade, as follows: 

    use Syscodes\Components\Support\Facades\DB;

    $users = DB::connection('mysql')->select(...)->get();

    return view('example', ['users' => $users]);