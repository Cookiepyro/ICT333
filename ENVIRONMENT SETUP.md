#Environement Setup
This document will guide you in setting up the development environment that will be used in the development of **WILSys**

##GIT
The development codes will be checked into *Github*. And as such *Git* and the *Hub* extension will be used.

First we will install *Git*. Type the following:
```
ICT333@devbox~:$ sudo apt-get install git
```

Next we will install the *Hub* extension for *Git*.
```
ICT333@devbox~:$ git clone https://github.com/github/hub.git
ICT333@devbox~:$ cd hub
ICT333@devbox~/hub:$ git checkout 1.12-stable
ICT333@devbox~/hub:$ rake install prefix=/usr/local
ICT333@devbox~/hub:$ $ alias git=hub
```

Check that both *Git* and *Hub* were properly installed by typing:
```
ICT333@devbox~:$ git version
```

And you should see:
```
git version 1.9.1
hub version 1.12.0
```

###PHP
PHP 5 should already be installed in your Ubuntu 14.04 installation by default. To check that PHP 5 has been installed type:  
```
ICT333@devbox:~$ php -v
```

And you should get:  
```
PHP 5.5.9-1ubuntu4.4 (cli) (built: Sep  4 2014 06:56:34) 
Copyright (c) 1997-2014 The PHP Group
Zend Engine v2.5.0, Copyright (c) 1998-2014 Zend Technologies
    with Zend OPcache v7.0.3, Copyright (c) 1999-2014, by Zend Technologies
```

If you do not, install PHP5 by typing:
```
ICT333@devbox:~$ sudo apt-get install php5
```

Confirm that it has been installed by typing:
```
ICT333@devbox:~$ php -v
```

###Composer
Composer is a PHP modules manager and is used by *Laravel 4* to miantain it's modules and packages.
To install *Composer* type:
```
ICT333@devbox:~$ curl -sS https://getcomposer.org/installer | php
ICT333@devbox:~$ mv composer.phar /usr/local/bin/composer
```

###Laravel
Laravel is a PHP framework. *WILSys* will be built on top of *Laravel 4*.
To install Larvel 4 download the following file.
[Laravel 4](https://github.com/laravel/laravel/archive/master.zip)  

Unzip the file and copy the contents to folder named public_html in your *Home* directory.
```
ICT333@devbox:~$ mkdir public_html
ICT333@devbox:~$ cp -r Downloads/laravel-master/* public_html/
```

Next run composer to install the dependecies.
```
ICT333@devbox:~$ cd public_html
ICT333@devbox:~/public_html$ composer install
```

###Apache
Apache 2 should already be installed in your Ubuntu 14.04 installation by default. To check that Apache 2 has been installed type:  
```
ICT333@devbox:~$ apache2 -v
```

And you should get:  
```
Server version: Apache/2.4.7 (Ubuntu)
Server built:   Jul 22 2014 14:36:38
```

If you do not, install Apache 2 by typing:
```
ICT333@devbox:~$ sudo apt-get install apache2
```

Confirm that it has been installed by typing:
```
ICT333@devbox:~$ apache2 -v
```

We now need to configure *Laravel 4* to work with *Apache 2*. Type in the following:
```
ICT333@devbox:~$ cd public_html
ICT333@devbox:~public_html$ chown -R :www-data app/storage
ICT333@devbox:~public_html$ chmod -R 775 app/storage
```

Finally we make the *Laravel 4* installation visible to *Apache 2*.
```
ICT333@devbox:~$ cd /etc/apache2/sites-available
ICT333@devbox:/etc/apache2/sites-available$ cp 000-default.conf ict333.conf
```

Open the file *ICT333.conf* and edit the section *<VirtualHost *:80>* by changing *DocumentRoot*, change _user_ to your _username_.
```
  ServerAdmin webmaster@localhost  
  DocumentRoot /home/user/public_html/public
```

Add the following section after *DocumentRoot*.
```
 <Directory /home/user/public_html/public>  
      Options Indexes FollowSymLinks  
      AllowOverride All  
      Require all granted  
</Directory>  
```

Now tell *Apache 2* to serve the *Laravel 4* web app by typing:
```
ICT333@devbox:~$ sudo a2ensite ict333
ICT333@devbox:~$ sudo apache2 restart
```
