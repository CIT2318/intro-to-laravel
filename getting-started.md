# Installing Laravel

The official installation instructions for laravel are very good - https://laravel.com/docs, but can be a bit daunting for beginners.

There are also a seri
es of videos for learning Laravel on Laracasts. The first video covers installation: https://laracasts.com/series/laravel-6-from-scratch.

The following explains how to get up and running with Laravel if you are using XAMPP, like we are in this module. Here are a couple of things to note before we get started:
* You can't do this using selene. The latest version of Laravel uses PHP 7.2, we only have version 7.0 on selene. 

## Installing Composer

First we need to download composer. Click on the following link.
```
https://getcomposer.org/composer.phar
```
* Save the file into the PHP folder of XAMPP
* From the XAMPP control panel click 'shell'
* A command line shell should open. Enter:
```
cd php
```
Then enter
```
dir
```
The list of all the files and folders in the php folder will be shown. Make sure you can see the *composer.phar* file you have just downloaded.
* To make Composer accessible we need to create a .bat file.  Enter the following carefully into the XAMPP shell (this generates a batch file)
```
echo @php "%~dp0composer.phar" %*>composer.bat
```
Now enter
```
composer
```
If it has installed correctly, the above will list all the composer commands

* You can now close the shell window

## Installing laravel
* Open the XAMPP shell
* Change directory to the htdocs folder
```
cd htdocs
```
* To install laravel enter the following composer command (specifiying the name of your project)
```
composer create-project --prefer-dist laravel/laravel name-of-your-project
```
* Composer should now download Laravel and download Laravel's dependencies (this may take a bit of time)
* Once this has finished. Open a web browser and enter http://localhost/name-of-your-project/public/ and you should see the default laravel page

### What else can go wrong?

Depending on which version of XAMPP you have installed you may need to enable some PHP extensions. See https://laravel.com/docs/installation for server requirements. If you unsure how to enable extensions please ask.
