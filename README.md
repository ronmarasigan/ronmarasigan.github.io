# LavaLust Framework Documentation

## Server Requirements
	* At least use PHP 7.2
	* MySQL 5 or higher
	* PDO is installed
	* mod_rewrite is enabled

## LavaLust is installed in four steps:
	1. Unzip the package or you can also clone its repository in Github
	2. Upload the LavaLust folders and files to your server.
	3. Open the application/config/config.php file with a text editor and set your base URL.
	4. If you intend to use a database, open the application/config/database.php file with a text editor and set your
	database settings..

	To add more flavor in your security, you can change the $system_path and $application_folder name to something new. You can do
	this by changing their values inside index.php. You can all place them outside public_html directory provided that you will use
	the full path, e.g. ‘/www/MyUser/system’.

	Hooray! Enjoy your new LavaLust Installation

## Features
	* Model-View-Controller Patter
	* Super Light Weight
	* Simple Query Builder Class
	* Form and Data Validation
	* Security and XSS Filtering
	* Session Management
	* Caching
	* Search-engine Friendly URLs
	* Many more...

## MVC
	Lavalust uses MVC patter. MVC is short for Model, View, and Controller. MVC is a popular way of organizing your code. The big
	idea behind MVC is that each section of your code has a purpose, and those purposes are different. Some of your code holds the
	data of your app, some of your code makes your app look nice, and some of your code controls how your app functions.

## Tutorial
	This section will only introduce you to basic principles of MVC using Lavalust.


