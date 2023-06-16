# Hashing

- [Introduction](#introduction)
- [Configuration](#configuration)
- [Basic Usage](#basic-usage)
    - [Hashing Passwords](#hashing-passwords)
    - [Verifying A Password Matches A Hash](#verifying-password-matches-hash)
    - [Verifying If A Password Needs To Be Rehashed](#verifying-password-needs-to-be-rehashed)

<a name="introduction"></a>
## Introduction

The Lenevor `Hash` facade provides secure Bcrypt and Argon2 hashing for storing user passwords. Bcrypt will be used for registration and authentication by default.

Bcrypt is a great choice for hashing passwords because its "work factor" is adjustable, which means that the time it takes to generate a hash can be increased as hardware power increases.

<a name="configuration"></a>
## Configuration

The default hashing driver for your application is configured in your application's `config/hashing.php` configuration file. There are currently several supported drivers: [Bcrypt](https://en.wikipedia.org/wiki/Bcrypt) and [Argon2](https://en.wikipedia.org/wiki/Argon2) (Argon2i and Argon2id variants).

<a name="basic-usage"></a>
## Basic Usage 

<a name="hashing-passwords"></a>
### Hashing Passwords

You may hash a password by calling the `make` method on the `Hash` facade, as follow:

    <?php

    namespace App\Http\Controllers;

    use App\Models\User;
    use App\Http\Controller;
    use Syscodes\Components\Http\Request;
    use Syscodes\Components\Support\Facades\Hash;
    use Syscodes\Components\Http\RedirectResponse;

    class LoginController extends Controller
    {
        /**
         * Update the password for the user.
         * 
         * @param  \Syscodes\Components\Http\Request  $request
         *
         * @return \Syscodes\Components\Http\RedirectResponse
         */
        public function update(Request $request): RedirectResponse
        {
            // Validate the new password length...
            
            (new User)->fill([
                'name' => 'Alexander',
                'email' => 'alexander@example.com',
                'password' => Hash::make($request->password)
            ])->save();

            return redirect('/profile');
        }
    }

<a name="bcrypt-work-factor"></a>
#### The Bycrypt Work Factor

If you are using the Bcrypt algorithm, the `make` method allows you to manage the work factor of the algorithm using the `rounds` option; however, the default work factor managed by Lenevor is acceptable for most applications, as follow:

    $hash = Hash::make('password', [
                'rounds' => 12,
            ]);

<a name="argon2-work-factor"></a>
#### The Argon2 Work Factor

If you are using the Argon2 algorithm, the `make` method allows you to manage the work factor of the algorithm using the `memory`, `time`, and `threads` options; however, the defaults are acceptable for most applications, as follow:

    $hash = Hash::make('password', [
                'memory' => 1024,
                'time'   => 2,
                'threads => 2,
            ]);

> **Note**  
> For more information on these options, please refer to the [official PHP documentation regarding Argon hashing](https://secure.php.net/manual/en/function.password-hash.php).

<a name="verifying-password-matches-hash"></a>
### Verifying A Password Matches A Hash

The `check` method provided by the `Hash` facade allows you to verify that a given plain-text string corresponds to a given hash, as follow:

    if (Hash::check('plain-text', $hashedValue)) {
        // The passwords match...
    }

<a name="verifying-password-needs-to-be-rehashed"></a>
### Verifying If A Password Needs To Be Rehashed

The `needsRehash` method provided by the `Hash` facade allows you to determine if the work factor used by the hasher has changed since the password was hashed. Some applications choose to perform this check during the application's authentication process, as follow:

    if (Hash::needsRehash($hash)) {
        $hash = Hash::make('plain-text');
    }
