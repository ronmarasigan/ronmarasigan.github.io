# LavaLust Framework Documentation

	LavaLust is an lightweight Web Framework - (using MVC pattern) - for people who are developing web sites using PHP. It helps
	you write code easily using Object-Oriented Approach. It also provides set of libraries for commonly needed tasks, as well as
	a helper functions to minimize the amount of time coding.

	LavaLust is only in its early release. You may encounter errors/bugs so please inform us creating issues and pull request
	in the github repository.

## Server Requirements
	* At least use PHP 7.2
	* MySQL 5 or higher
	* PDO is installed
	* mod_rewrite is enabled

## LavaLust is installed in four steps:
	1. Unzip the package or you can also clone its repository in Github
	Link: [https://github.com/ronmarasigan/LavaLust](https://github.com/ronmarasigan/LavaLust)
	2. Upload the LavaLust folders and files to your server.
	3. Open the application/config/config.php file with a text editor and set your base URL.
		$base_url = 'http://localhost/path_to_your_folder/' or
		$base_url = 'http://yourwebsite.com/
	4. If you intend to use a database, open the application/config/database.php file with a text editor and set your
	database settings..

	To add more flavor in your security, you can change the $system_path and $application_folder name to something new. You can
	do this by changing their values inside index.php. You can all place them outside public_html directory provided that you
	will use the full path, e.g. ‘/www/MyUser/system’.

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

	We might imagine that there is a controller named “users”. The method being called on users would be “profile”. The profile
	method’s job could be to grab user's profile number 1, and render them on the page. Very often in MVC, you’ll see URL patterns
	that match:

	http://site.com/[controller-class]/[controller-method]/[arguments]

	As URL schemes become more complex, this may change. But for now, this is all we will need to know.

	Create a file at application/controllers/Users.php with the following code.

```php
<?php
class Users extends Controller {

        public function profile($profile)
        {
        }
}
```

	You have created a class named Users, with a profile method that accepts one argument named $profile. The Pages class is
	extending the Controller class. This means that the new pages class can access the methods and variables defined in the
	Controller class (system/core/Controller.php).

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

	Final Code will be:
	
```php
<?php
class Users extends Controller {

        public function profile($profile)
        {
        	$this->load->view('profile');
        }
}
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

	Inside this view file we can access $data variable through this example.

```php
<html>
<head>
	<title>Profile</title>
</head>
<body>
	This is the content!
	<?php echo $data['profile']; ?>
</body>
</html>
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

	Open up the application/models/ directory and create a new file called Sample_model.php and add the following code.

	CREATE TABLE users (
        id int(11) NOT NULL AUTO_INCREMENT,
        username varchar(20) NOT NULL,
        email varchar(60) NOT NULL,
        PRIMARY KEY (id)
	);

	Populate the table then perform the following codes.

```php
<?php
class Sample_model extends Model {

	$private $db;

    public function __construct()
    {
		$this->$db = $this->load->database();
    }

    public function loadusers() {
    	return $this->db->table('users')->get_all();
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

## Database (Query Builder)

## List of Methods

### Insert
```php
$bind = array(
	'username' => $username,
	'password' => $this->passwordhash($password),
	'email' => $email,
	'usertype' => $usertype,
	);

$this->db->table('user')->insert($bind)->exec();
```

### update
```php
$data = [
	'username' => 'ronmarasigan',
	'password' => 'pass',
	'activation' => 1,
	'status' => 1
];

$this->db->table('users')->where('id', 10)->update($data)->exec();
# Output: "UPDATE users SET username='ronmarasigan', password='pass', activation='1', status='1' WHERE id='10'"
```

### delete
```php
$this->db->table('table')->where("id", 17)->delete()->exec();
# Output: "DELETE FROM table WHERE id = '17'"

# OR

$this->db->table('table')->delete()->exec();
# Output: "TRUNCATE TABLE delete"
```

### transaction
```php
$this->db->transaction();

$data = [
	'title' => 'new title',
	'status' => 2
];
$this->db->table('table')->where('id', 10)->update($data)->exec();

$this->db->commit();
# OR
$this->db->rollBack();
```

### Select
```php
# Usage 1: string parameter
$this->db->select('column1, column2')->table('table')->get_all();
# Output: "SELECT column1, column2 FROM table"

$this->db->select('column1 AS c1, column2 AS c2')->table('table')->get_all();
# Output: "SELECT column1 AS c1, column2 AS c2 FROM table"
```

### select functions (min, max, sum, avg, count)
```php
# Usage 1:
$this->db->table('table')->select_max('price')->get();
# Output: "SELECT MAX(price) FROM table"

# Usage 2:
$this->db->table('table')->select_count('id', 'total_row')->get();
# Output: "SELECT COUNT(id) AS total_row FROM table"
```

### table
```php
### Usage 1: string parameter
$this->db->table('table');
# Output: "SELECT * FROM table"

$this->db->table('table1, table2');
# Output: "SELECT * FROM table1, table2"

$this->db->table('table1 AS t1, table2 AS t2');
# Output: "SELECT * FROM table1 AS t1, table2 AS t2"
```

### get AND get_all
```php
# get(): return 1 record. It has one optional parameter, yo u can set the fetchmode. example: PDO::FETCH_OBJ by
# default PDO::FETCH_ASSOC
# getAll(): return multiple records.

$this->db->table('table')->get_all(); 
# Output: "SELECT * FROM table"

$this->db->select('username')->table('users')->where('status', 1)->get_all();
# Output: "SELECT username FROM users WHERE status='1'"

$this->db->select('title')->table('pages')->where('id', 17)->get(); 
# Output: "SELECT title FROM pages WHERE id='17' LIMIT 1"
```

### join
```php
$this->db->table('test as t')->join('foo as f', 't.id', 'f.t_id')->where('t.status', 1)->get_all();
# Output: "SELECT * FROM test as t JOIN foo as f ON t.id=f.t_id WHERE t.status='1'"
```
You can use this method in 7 ways. These;
- join
- left_join
- right_join
- inner_join
- full_outer_join
- left_outer_join
- right_outer_join

Examples:
```php
$this->db->table('test as t')->left_join('foo as f', 't.id=f.t_id')->get_all();
# Output: "SELECT * FROM test as t LEFT JOIN foo as f ON t.id=f.t_id"
```

```php
$this->db->table('test as t')->full_outer_join('foo as f', 't.id=f.t_id')->get_all();
# Output: "SELECT * FROM test as t FULL OUTER JOIN foo as f ON t.id=f.t_id"
```

### where
```php
$where = [
	'name' => 'Ron',
	'age' => 23,
	'status' => 1
];
$this->db->table('users')->where($where)->get();
# Output: "SELECT * FROM users WHERE name='Ron' AND age='23' AND status='1' LIMIT 1"

# OR

$this->db->table('users')->where('active', 1)->get_all();
# Output: "SELECT * FROM users WHERE active='1'"

# OR

$this->db->table('users')->where('age', '>=', 18)->getAll();
# Output: "SELECT * FROM users WHERE age>='18'"

# OR

$this->db->table('users')->where('age = ? OR age = ?', [18, 20])->getAll();
# Output: "SELECT * FROM users WHERE age='18' OR age='20'"
```

You can use this method in 4 ways. These;

- where
- or_where
- not_where
- or_not_where
- where_null
- where_not_null

Example:
```php
$this->db->table('users')->where('active', 1)->not_where('auth', 1)->get_all();
# Output: "SELECT * FROM users WHERE active = '1' AND NOT auth = '1'"

# OR

$this->db->table('users')->where('age', 20)->or_where('age', '>', 25)->get_all();
# Output: "SELECT * FROM users WHERE age = '20' OR age > '25'"

$this->db->table('users')->where_not_null('email')->get_all();
# Output: "SELECT * FROM users WHERE email IS NOT NULL"
```

### grouped
```php
$this->db->table('users')
	->grouped(function($q) {
		$q->where('country', 'USA')->or_where('country', 'Philippines');
	})
	->where('status', 1)
	->get_all();
# Ouput: "SELECT * FROM users WHERE (country='USA' OR country='Philippines') AND status ='1'"
```

### in
```php
$this->db->table('users')->where('active', 1)->in('id', [1, 2, 3])->getAll();
# Output: "SELECT * FROM users WHERE active = '1' AND id IN ('1', '2', '3')"
```

You can use this method in 4 ways. These;

- in
- or_in
- not_in
- or_not_in

Example:
```php
$this->db->table('users')->where('active', 1)->not_in('id', [1, 2, 3])->get_all();
# Output: "SELECT * FROM users WHERE active = '1' AND id NOT IN ('1', '2', '3')"

# OR

$this->db->table('users')->where('users', 1)->or_in('id', [1, 2, 3])->get_all();
# Output: "SELECT * FROM table WHERE active = '1' OR id IN ('1', '2', '3')"
```

### between
```php
$this->db->table('users')->where('active', 1)->between('age', 18, 25)->get_all();
# Output: "SELECT * FROM users WHERE active = '1' AND age BETWEEN '18' AND '25'"
```

You can use this method in 4 ways. These;

- between
- or_between
- not_between
- or_not_between

Example:
```php
$this->db->table('users')->where('active', 1)->not_between('age', 18, 25)->get_all();
# Output: "SELECT * FROM users WHERE active = '1' AND age NOT BETWEEN '18' AND '25'"

# OR

$this->db->table('users')->where('active', 1)->or_between('age', 18, 25)->get_all();
# Output: "SELECT * FROM users WHERE active = '1' OR age BETWEEN '18' AND '25'"
```

### like
```php
$this->db->table('books')->like('title', "%php%")->get_all();
# Output: "SELECT * FROM books WHERE title LIKE '%php%'"
```

You can use this method in 4 ways. These;

- like
- or_like
- not_like
- or_not_like

Example:
```php
$this->db->table('books')->where('active', 1)->not_like('tags', '%dot-net%')->get_all();
# Output: "SELECT * FROM books WHERE active = '1' AND tags NOT LIKE '%dot-net%'"

# OR

$this->db->table('books')->like('bio', '%php%')->or_like('bio', '%java%')->get_all();
# Output: "SELECT * FROM books WHERE bio LIKE '%php%' OR bio LIKE '%java%'"
```

### group_by
```php
# Usage 1: One parameter
$this->db->table('books')->where('status', 1)->group_by('cat_id')->get_all();
# Output: "SELECT * FROM books WHERE status = '1' GROUP BY cat_id"
```

```php
# Usage 1: Array parameter
$this->db->table('books')->where('status', 1)->groupBy(['cat_id', 'user_id'])->get_all();
# Output: "SELECT * FROM books WHERE status = '1' GROUP BY cat_id, user_id"
```

### having
```php
$this->db->table('books')->where('status', 1)->group_by('city')->having('COUNT(person)', 100)->get_all();
# Output: "SELECT * FROM books WHERE status='1' GROUP BY city HAVING COUNT(person) > '100'"

# OR

$this->db->table('employee')->where('active', 1)->group_by('department_id')->having('AVG(salary)', '<=', 500)->get_all();
# Output: "SELECT * FROM employee WHERE active='1' GROUP BY department_id HAVING AVG(salary) <= '500'"

# OR

$this->db->table('employee')->where('active', 1)->group_by('department_id')->having('AVG(salary) > ? AND MAX(salary) < ?', [250, 1000])->get_all();
# Output: "SELECT * FROM employee WHERE active='1' GROUP BY department_id HAVING AVG(salary) > '250' AND MAX(salary) < '1000'"
```

### order_by
```php
# Usage 1: One parameter
$this->db->table('test')->where('status', 1)->order_by('id', 'ASC')->get_all();
# Output: "SELECT * FROM test WHERE status='1' ORDER BY id ASC"
```

### limit - offset
```php
# Usage 1: One parameter
$this->db->table('table')->limit(10)->get_all();
# Output: "SELECT * FROM table LIMIT 10"

# Usage 2: with offset method
$this->db->table('test')->limit(10)->offset(10)->get_all();
# Outp
```

### Load Libraries and Helpers
	LavaLust has different library classes and helper functions to build your application easily. You can load the just what we 
	did in view and model.

	$this->load->library()
		see library class
	$this->load->helper()
		see helper functions

	To see all tutorials, You can check them in my Youtube Channel
[Youtube Channel] https://youtube.com/ronmarasigan

### License
	MIT License

	Copyright (c) 2020 Ronald M. Marasigan

	Permission is hereby granted, free of charge, to any person obtaining a copy
	of this software and associated documentation files (the "Software"), to deal
	in the Software without restriction, including without limitation the rights
	to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
	copies of the Software, and to permit persons to whom the Software is
	furnished to do so, subject to the following conditions:

	The above copyright notice and this permission notice shall be included in all
	copies or substantial portions of the Software.

	THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
	IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
	FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
	AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
	LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
	OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
	SOFTWARE.


