#Installing Laravel at the University

A couple of things before we start. 
* You will need XAMPP running on a USB stick. We can't do this on selene. 
* These instructions are specifically aimed at installing Laravel at University.  

> If you want to install composer on your home machine. You have a number of options
> 
> 1. You can use Homestead - a 'virtual machine'. 
> Virtual machines are worth knowing about if you are keen on web development, but you don't need to use Homestead to work with Laravel.   
> 
> 2. Just use Composer. This is the easier option. You can always look at Homestead when you have more time or a bit more experiences. 
> 
> Make sure you have PHP installed (if you installed WAMP or MAMP you will do). Then follow the instructions from https://getcomposer.org/download/ and then follow the isntructions from https://laravel.com/docs/5.4#installation.


##Installing Composer

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

##Installing laravel

* Visit https://github.com/laravel/laravel 
* Download a copy of laravel, unzip the folder, and move it onto your htdocs folder.
* You might want to change the name from *laravel-master*

Laravel has lots of dependencies (it relies on lots of other PHP libraries) but these aren't stored as part of its github repository. We will instruct Composer to download all this library code so that Laravel will work for us. 

* In a text editor open the file *composer.json* (it is in the root of the laravel folder)
* Add a new line just before the final closing curly bracket
* Paste in following into this gap:
```
,
    "repositories": [
    {
         "type": "composer", 
         "url": "https://packagist.org"
    },
    { "packagist": false }
]
```
* Save composer.json
* All this does is force composer to use https instead of http when retrieving files (we need to do this because of proxy settings at the University)
* Open the xampp shell again
* Change the shell so we are viewing the Laravel folder e.g.

```
cd htdocs/laravel-master
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

Once all the packages have finished downloading and installing. Open a web browser and enter http://localhost/laravel-project-name/public/ .You will get a laravel generated error message. This is because we haven't set up encryption. To do this. 

* Rename the file *.env.example* to *.env*

* In the xampp shell enter
```
php artisan key:generate
```

This generates a key that is used to encrypt data e.g. for session data. Once you have done this, refresh the page in a browser and you should see Laravel in big letters. This means the installation has worked. You are ready to use Laravel. 

###What else can go wrong?

Depending on which version of XAMPP you have installed you may need to enable some PHP extensions. See https://laravel.com/docs/5.4/installation for server requirements. You may need to enable a PHP extension. If you unsure how to enable extensions please ask in a practical session. 


