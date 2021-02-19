# Deployment

- [Introduction](#introduction)
- [Server Requeriments](#server-requeriments)
- [Server configuration](#server-configuration)
    - [Nginx](#nginx)
    - [Apache](#apache)
    - [IIS](#iis)
- [Debug Mode](#debug-mode)
- [Recommended](#recommended)

<a name="introduction"></a>
## Introduction

When you're ready to deploy your Lenevor application to production, there are many important things you can to make sure your application is running as efficiently as possible. In this document, it provides you with some excellent starting points to ensure that your Lenevor application is deployed properly. 

<a name="server-requeriments"></a>
## Server Requeriments

The Lenevor framework has some system requirements. You should ensure that your web server has the latest versions and minimum PHP extensions: 

<div class="content-list">
- PHP >= 7.3
- Fileinfo PHP Extension
- JSON PHP Extension
- Mbstring PHP Extension
- OpenSSL PHP Extension
- PDO PHP Extension
- XML PHP Extension
</div>

<a name="server-configuration"></a>
## Server configuration

<a name="nginx"></a>
### Nginx

If you are deploying your application to a server running Ngnix, you may use the following configuration file as a first step to configure your web server. Most likely, this file will need to be customized depending on your server's configuration:

    server {
        listen 80;
        server_name project.com;
        root /srv/project.com/public;

        access_log /var/www/access.log;
        error_log  /var/www/error.log;

        index  index.php;

        charset utf-8;

        location / {
            try_files $uri $uri/ /index.php?$args;
        }

        location = /favicon.ico { access_log off; log_not_found off; }
        location = /robots.txt {access_log off; log_not_found off;}
        location ~ /\\. {deny all; access_log off; log_not_found off;}

        error_page 404 /index.php;

        location ~ \.php$ {
            fastcgi_pass unix:/var/run/php7.4-fpm.sock;
            fastcgi_split_path_info ^(.+\.php)(/.+)$;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include fastcgi_params;
        }
    }

<a name="apache"></a>
## Apache

This is configuration del archivo `.htaccess`:

    <IfModule mod_rewrite.c>
        <IfModule mod_negotiation.c>
            Options -MultiViews -Indexes
        </IfModule>

        RewriteEngine On

        # Handle Authorization Header
        RewriteCond %{HTTP:Authorization} .
        RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]

        # Redirect Trailing Slashes If Not A Folder...
        RewriteCond %{REQUEST_FILENAME} !-d
        RewriteCond %{REQUEST_URI} (.+)/$
        RewriteRule ^ %1 [L,R=301]

        # Send Requests To Front Controller...
        RewriteCond %{REQUEST_FILENAME} !-d
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteRule ^ index.php [L]    
    </IfModule>

<a name="iis">
### IIS

For IIS, Lenevor provides a `web.config` file located in public/web.config:

    <configuration>
        <system.webServer>
            <rewrite>
                <rules>
                    <rule name="Imported Rule 1" stopProcessing="true">
                        <match url="^(.*)/$" ignoreCase="false" />
                        <conditions>
                            <add input="{REQUEST_FILENAME}" matchType="IsDirectory" ignoreCase="false" negate="true" />
                        </conditions>
                        <action type="Redirect" redirectType="Permanent" url="/{R:1}" />
                    </rule>
                    <rule name="Imported Rule 2" stopProcessing="true">
                        <match url="^" ignoreCase="false" />
                        <conditions>
                            <add input="{REQUEST_FILENAME}" matchType="IsDirectory" ignoreCase="false" negate="true" />
                            <add input="{REQUEST_FILENAME}" matchType="IsFile" ignoreCase="false" negate="true" />
                        </conditions>
                        <action type="Rewrite" url="index.php" />
                    </rule>
                </rules>
            </rewrite>
        </system.webServer>
    </configuration>

<a name="debug-mode">
## Debug Mode

For the debug option in your configuration file `config/app.php` it determines what information should be shown about an error really to the user. By default, this option is set to activate the value of the environment variable `APP_DEBUG`, which is found in your .env file of the root folder.

** In production environment, this variable value must be `false`. If the `APP_DEBUG` variable is set to` true` in production, you risk exposing configuration values ​​to your application's end users. ** 

<a name="recommended">
## Recommended

This framework was designed to be installed above the document root directory, with the document root being the public (commonly this is a known as public_html folder) folder.

Additionally, installing in a sub-directory, on a production server, will introduce severe security issues. If there is no choice still place the framework files above the document root and have only index.php and .htacess from the public folder in the sub folder and adjust the paths in index.php.