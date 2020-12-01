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
	this by changing their values inside index.php. You can all place them outside public_html directory provided that you will
	use the full path, e.g. ‘/www/MyUser/system’.

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

	Let say we have this link

	http://site.com/users/profile/1

	We might imagine that there is a controller named “users”. The method being called on news would be “users”. The news method’s
	job could be to grab profile number 1, and render them on the page. Very often in MVC, you’ll see URL patterns that match:

	http://site.com/[controller-class]/[controller-method]/[arguments]

	As URL schemes become more complex, this may change. But for now, this is all we will need to know.

	Create a file at application/controllers/Users.php with the following code.

```php
class Users extends Controller {

        public function profile($arg = 1)
        {
        }
}
```

	You have created a class named Users, with a profile method that accepts one argument named $arg. The Pages class is extending
	the Controller class. This means that the new pages class can access the methods and variables defined in the Controller
	class (system/core/Controller.php).

	The controller is what will become the center of every request to your web application. In very technical LavaLust
	discussions, it may be referred to as the super object. Like any php class, you refer to it within your controllers
	as $this. Referring to $this is how you will load libraries, views, and generally command the framework.

	Now you’ve created your first method, it’s time to make some basic page templates inside the folder "application/views/". You just name it "profile.php";

	Here's the code:

```html
<html>
<head>
	<title>Profile</title>
</head>
<body>
	This is the content!
</body>
</html>
```


