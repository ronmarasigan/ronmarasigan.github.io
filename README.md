# LavaLust Framework Documentation

	LavaLust is an lightweight Web Framework - (using MVC pattern) - for people who are developing web sites using PHP. It helps
	you write code easily using Object-Oriented Approach. It also provides set of libraries for commonly needed tasks, as well as
	a helper functions to minimize the amount of time coding.

	LavaLust is only in its early release. You may encounter errors/bugs so please inform us creating issues and pull request in the github repository.

## Server Requirements
	* At least use PHP 7.2
	* MySQL 5 or higher
	* PDO is installed
	* mod_rewrite is enabled

## LavaLust is installed in four steps:
	1. Unzip the package or you can also clone its repository in Github
	2. Upload the LavaLust folders and files to your server.
	3. Open the application/config/config.php file with a text editor and set your base URL.
		$base_url = 'http://localhost/your_folder/' or
		$base_url = 'http://yourwebsite.com/
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
<?php
class Users extends Controller {

        public function profile($profile = 'RON')
        {
        }
}
```

	You have created a class named Users, with a profile method that accepts one argument named $profile. The Pages class is extending
	the Controller class. This means that the new pages class can access the methods and variables defined in the Controller
	class (system/core/Controller.php).

	The controller is what will become the center of every request to your web application. In very technical LavaLust
	discussions, it may be referred to as the super object. Like any php class, you refer to it within your controllers
	as $this. Referring to $this is how you will load libraries, views, and generally command the framework.

	Now you’ve created your first method, it’s time to make some basic page templates inside the folder application/views/.
	You just name it "profile.php";

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

#### Adding logic to the controller
	Earlier you set up a controller with a view() method. The method accepts one parameter, which is the Profile name to be
	printed. The static page templates will be located in the application/views/ directory.

```php
public function profile($profile = 'RON')
{
	$data['profile'] = $profile;
	$this->load->view('profile', $data); 
}
```

	The profile name is defined in this method, but instead of assigning the value to a variable, it is assigned to the profile
	element in the $data array.

	The last thing that has to be done is loading the views in the order they should be displayed. The second parameter in the
	view() method is used to pass values to the view. Each value in the $data array is assigned to a variable with the name of
	its key. So the value of $data['profile'] in the controller is equivalent to $profile in the view.

### Load Model
	Instead of writing database operations right in the controller, queries should be placed in a model, so they can easily be
	reused later. Models are the place where you retrieve, insert, and update information in your database or other data stores.
	They represent your data.

	Open up the application/models/ directory and create a new file called News_model.php and add the following code.

	CREATE TABLE users (
        id int(11) NOT NULL AUTO_INCREMENT,
        username varchar(20) NOT NULL,
        email varchar(60) NOT NULL,
        PRIMARY KEY (id)
	);

```php
<?php
class Sample_model extends Model {

	$private $db;

    public function __construct()
    {
		$this->$db = $this->load->database();
    }

    public function loadusers() {
    	return $this->db->table('users')->fetchAll();
    }
}
```
	This code looks similar to the controller code that was used earlier. It creates a new model by extending
	Model and loads
	the database library. This will make the database class available through the $this->db object.

	Also using Query builder of LavaLust, you can create queries easier as what the example method loadusers is done.

	To display the all the output from __loadusers__ method you can user the Users class we have created earlier.

```php
class Users extends Controller {
	public function viewusers() {
		$users = $this->load->model('loadusers');

		var_dump($users);
	}
}
```

	This will show all the output coming from loadusers method. You can also load view to display the output.

### Load Library and Helpers
	LavaLust has different library classes and helper functions to build your application easily. You can load the just what we 
	did in view and model.

	$this->load->library()
		see library class
	$this->load->helper()
		see helper functions

	To see all tutorials, You can check them in my Youtube Channel
[TechRON](https://www.youtube.com/ronmarasigan)

### License


