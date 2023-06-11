# Encryption

- [Introduction](#introduction)
- [Configuration](#configuration)
- [Using The Encrypter](#using-encrypter)

<a name="introduction"></a>
## Introduction

Lenevor's encryption services offer you a simple, secure interface for encrypting and decrypting text via OPENSSL using AES-256 and AES-128 encryption. These Lenevor's encrypted values are signed with a message authentication code (MAC) so that the underlying value can not be modified or altered once encrypted.

<a name="configuration"></a>
## Configuration

In order using Lenevor's encrypter, you must set the key configuration option in your `config/app.php` configuration file. This configuration value is managed by the `APP_KEY` environment variable. You must use the `php prime key:generate` command to generate the value of this variable, as the `key:generate` command will create a PHP secure random byte generator to obtain a cryptographically secure key for your application. Typically, the value of the `APP_KEY` environment variable will be generated during [Lenevor's installation]('/installation.md').

<a name="using-encrypter"></a>
## Using The Encrypter

<a name="encrypting-value"></a>
#### Encrypting A Value

You may encrypt a value using the `encryptString` method provided by the `Crypt` facade. All encrypted values are encrypted using OpenSSL and the AES-256-CBC cipher. Also, all encrypted values are signed with a message authentication code (MAC). The integrated message authentication code will prevent the decryption of any values that have been tampered with by malicious users, as follow:

    // Path: app/Models/Encryption.php
    <?php>

    namespace App\Models;

    use Syscodes\Components\Database\Erostrine;

    class Encryption extends Model
    {
        protected $fillable = ['title', 'description'];
    }

    // Path: app/Http/Controllers/TestController.php
    <?php

    namespace App\Http\Controllers;

    use App\Http\Controller;
    use App\Models\Encryption;
    use Syscodes\Components\Http\Request;
    use Syscodes\Components\Http\RedirectResponse;
    use Syscodes\Components\Support\Facades\Crypt;

    class TestController extends Controller
    {
        public function saveEncryption(Request $request): RedirectResponse;
        {
            $encryption = new Encryption;

            $encryption->fill([
                'title' => Crypt::encryptString($request->title),
                'description' => Crypt::encryptString($request->description)
            ])->save();

            return redirect('/auth-secret');
        }
    }

<a name="decrypting-value"></a>
#### Decrypting A Value

You may decrypt values using the `decryptString` method provided by the `Crypt` facade. If the value can not be properly decrypted, such as when the message authentication code is invalid, an `Syscodes\Components\Encryption\Exceptions\DecryptException` will be thrown, as follow:

    use Syscodes\Components\Support\Facades\Crypt;
    use Syscodes\Components\Encryption\Exceptions\DecryptException;

    try {
        $decrypted = Crypt::decryptString($value);
    } catch (DecryptException $e) {
        // ...
    }