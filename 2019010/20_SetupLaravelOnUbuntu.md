# Setup Laravel PHP Framework on Ubuntu 18.04 LTS

## Install PHP 7.3

Use the following command to install PHP.

```bash
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt-get install -y php7.3
```

Now use the following command to check installed PHP version on your system.

```bash
php -v
```

## Install PHP 7 Modules

You may also need to install modules based on your application requirements. Use the following command to search available PHP 7 modules in the package repository.

```bash
sudo apt-cache search php7
```

You can install the required PHP modules on your system as below command. Make sure to install packages for correct PHP version by specifying the version with the package name. Without defining the package version, it will install the latest package.

```bash
sudo apt-get install php7.3-mysql php7.3-curl php7.3-json php7.3-cgi php7.3-xsl
```

The Laravel framework **version 6.x** has a few system requirements. All of these requirements are satisfied by the Laravel Homestead virtual machine. However, we are not using Homestead, so we will need to make sure our system meets the following requirements:

- PHP >= 7.2.0
- BCMath PHP Extension
- Ctype PHP Extension
- JSON PHP Extension
- Mbstring PHP Extension
- OpenSSL PHP Extension
- PDO PHP Extension
- Tokenizer PHP Extension
- XML PHP Extension

The following command will install those requirements:

```bash
apt-get install zip unzip php-pear php-token-stream php7.3-curl php7.3-dev php7.3-gd php7.3-mbstring php7.3-zip php7.3-mysql php7.3-xml  php7.3-json php7.3-bcmath
```

## Install Composer

To install Laravel, we need to install Composer first. It is a tool for dependency management in PHP that allows you to package all the required libraries associated with a package as one.

To install Laravel and all its dependencies, Composer is required. It will download and install everything that is required to run the Laravel framework. To install Composer, issue the following commands:

```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php composer-setup.php
php -r "unlink('composer-setup.php');"
sudo mv composer.phar /usr/local/bin/composer
```

The 4 lines above will, in order:

- Download the installer to the current directory.
- Run the installer.
- Remove the installer.
- Move PHAR archive to **usr/local/bin** to use it globally on our system.

## Install Laravel

First, download the Laravel installer using Composer.

```bash
composer global require "laravel/installer=~1.1"
```

Make sure to place the **~/.composer/vendor/bin** directory in your PATH so the `laravel` executable is found when you run the `laravel` command in your terminal.

The next thing to do is updating the laravel installer by running:

```bash
composer global update
```

Once installed, the simple `laravel new` command will create a fresh Laravel installation in the directory you specify. For instance, `laravel new blog` would create a directory named **blog** containing a fresh Laravel installation with all dependencies installed.

Go to the new created folder `blog`, and run the following command to start our first Laravel application:

```bash
cd blog
php artisan serve
```

Open your browser and go to <http://127.0.0.1:8000>. Here is what you see when everything is setup correctly!

![console](img/laravel_application.png?raw=true)
