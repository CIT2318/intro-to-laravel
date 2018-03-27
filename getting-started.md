# Installing Laravel

The official installation instructions for laravel are very good - https://laravel.com/docs/5.6, but can be a bit daunting for beginners.

There are also a series of videos for learning Laravel on Laracasts. The first video covers installation: https://laracasts.com/series/laravel-from-scratch-2017. 

The following explains how to get up and running with Laravel if you are using XAMPP, like we are in this module. Here are a couple of things to note before we get started:
* You can't do this using selene.
* We are going to use version 5.5 of laravel. This is so our site will still work when we upload it to selene. The latest version of Laravel uses PHP 7.1.3, we only have version 7.0 on selene. 

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

* Visit https://github.com/laravel/laravel/tree/5.5
* Download a copy of laravel, unzip the folder, and move it onto your htdocs folder. For now make sure you put it in the root of this folder. 
* You might want to change the name of the folder to something easy to work with e.g. *laravel*

Laravel has lots of dependencies (it relies on lots of other PHP libraries) but these aren't stored as part of its github repository. We will instruct Composer to download all this library code so that Laravel will work for us. 

* Open the xampp shell again
* Change the shell so we are viewing the Laravel folder e.g.

```
cd htdocs/laravel
```
Enter
```
dir
```
To check you are in the right folder. Enter 
```
composer install
```
You should get a message along the lines of *Loading composer repositories with package information...*. It will list the packages it is installing

* Wait - this can take while....

Once all the packages have finished downloading and installing. Start Apache. Open a web browser and enter http://localhost/laravel/public/ .You will get a laravel generated error message. This is because we haven't set up encryption. To do this. 

* Rename the file *.env.example* to *.env* (Open it and do a *save as*)

* In the xampp shell enter
```
php artisan key:generate
```

This generates a key that is used to encrypt data e.g. for session data. Once you have done this, refresh the page in a browser and you should see Laravel in big letters. This means the installation has worked. You are ready to use Laravel. 

### What else can go wrong?

Depending on which version of XAMPP you have installed you may need to enable some PHP extensions. See https://laravel.com/docs/5.5/installation for server requirements. If you unsure how to enable extensions please ask.


