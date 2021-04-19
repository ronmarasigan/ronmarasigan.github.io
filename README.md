# LavaLust Framework Documentation

	LavaLust is an lightweight Web Framework - (using MVC pattern) - for people who are developing web sites using PHP. It helps
	you write code easily using Object-Oriented Approach. It also provides set of libraries for commonly needed tasks, as well as
	a helper functions to minimize the amount of time coding.

	LavaLust is only in its early release. You may encounter errors/bugs so please inform us creating issues and pull request
	in the github repository.

	Note: If you are using LavaLust Version < 2 then you will use $this->load instead of $this->call. Default folder structure
	for version > 1 is app, scheme and public but the function is same with the older version.

## Server Requirements
	* At least use PHP 7.2
	* MySQL 5 or higher
	* PDO is installed
	* mod_rewrite is enabled

## LavaLust is installed in four steps:
	1. Unzip the package or you can also clone its repository in Github

Link: [LavaLust Framework](https://github.com/ronmarasigan/LavaLust)

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
        	$this->call->view('profile');
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
	$this->call->view('profile', $data); 
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

    public function loadusers() {
    	$this->call->database();
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
		$users = $this->call->model('loadusers');

		var_dump($users);
	}
}
```
	This will show all the output coming from loadusers method. You can also load view to display the output.

## Database (Query Builder)

## List of Methods

### Raw Query
```php
$this->db->raw('select * from users where username = ?', array($username));
# Output: "INSERT INTO users username VALUES ($username)"
```

### Insert
```php
$bind = array(
	'username' => $username,
	'password' => $this->passwordhash($password),
	'email' => $email,
	'usertype' => $usertype,
	);

$this->db->table('user')->insert($bind)->exec();
# Output: "INSERT INTO user username, password, email, usertype VALUES ($username, $password, $email, $usertype)"
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
$this->db->table('table')->select('column1, column2')->get_all();
# Output: "SELECT column1, column2 FROM table"

$this->db->table('table')->select('column1 AS c1, column2 AS c2')->get_all();
# Output: "SELECT column1 AS c1, column2 AS c2 FROM table"
```

### select functions (min, max, sum, avg, count)
You can use this method in 5 ways. These;

- select_min
- select_max
- select_sum
- select_avg
- select_count

Examples:
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

$this->db->table('users')->select('username')->where('status', 1)->get_all();
# Output: "SELECT username FROM users WHERE status='1'"

$this->db->table('pages')->select('title')->where('id', 17)->get(); 
# Output: "SELECT title FROM pages WHERE id='17' LIMIT 1"
```

### join
```php
$this->db->table('test as t')->join('foo as f', 't.id=f.t_id')->where('t.status', 1)->get_all();
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

$this->db->table('users')->where('age', '>=', 18)->get_all();
# Output: "SELECT * FROM users WHERE age>='18'"

# OR

$this->db->table('users')->where('age = ? OR age = ?', [18, 20])->get_all();
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
$this->db->table('books')->where('status', 1)->group_by(['cat_id', 'user_id'])->get_all();
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

## Load Libraries and Helpers

	LavaLust has different library classes and helper functions to build your application easily. You can load the just what we 
	did in view and model.


	$this->call->library()
		see library class
	$this->call->helper()
		see helper functions

### Session Library

```php
#Set / Initialize session
$sess = array(
	$username = 'acidcore',
	$usertype = 'ADMIN'
);
$this->session->set_userdata($sess);

#Access session variable
$this->session->userdata('session_variable')
#session_variable is the initialized variable from set_userdata()

#Set Flash data
$sess = array(
	$err_message = 'Something went wrong'
);
$this->session->set_flashdata($sess);

#Access Flash data
$this->session->flashdata('session_variable')
#session_variable is the initialized variable from set_flashdata()

#Unset session variable
$sess = array(
	$username = 'acidcore',
	$usertype = 'ADMIN'
);
$this->session->unset_userdata($sess);

#Destroy session
$this->session->sess_destroy();
```

### Cache Library

```php
#Uncached model call
$this->news->get_news($category, 'business');

#cached model call / cache for 2 minutes
$this->cache->model('news', 'get_news', array($category, 'business'), 120);

#cached library call / empty or 0 for the last parameter for unlimited time
$this->cache->library('some_library', 'calcualte_something', array($foo, $bar, $bla)); 

#Cached array or object
$this->cache->write($data, 'cached_name');
$data = $this->cache->get('cached_name');

#Delete cache
$this->cache->delete('cached_name');

#Delete all cache
$this->cache->delete_all();

#Delete cache group
$this->cache->write($data, 'nav_header');
$this->cache->write($data, 'nav_footer');
$this->cache->delete_group('nav_');

#Delete cache item
#Call like a normal library or model but give a negative $expire
$this->cache->model('news', 'get_news', array($category, 'business'), -1);
```

### Email Library

```php
#Email Subject
$this->email->subject('Email Subject')

#Sender
$this->email->sender($sender_email);
#where $sender_email is the email of the sender

#Email content / Plain or HTML / Default: Plain
$this->email->email_content($content, 'html');
#where $content is the email content

#Attachment
$this->email->attachment('attachment link or path');

#Recipient
$this->email->recipient($recipient_email);
#where $recipient_email is the email of the recipient

#Reply to
$this->email->recipient($replyto_email);
#If your want to specify other email to receive the reply

#Send
$this->email->send();
```

### Form Validation Class

```php
#Check if form was submitted with content
$this->form_validation->submitted();

#Name
$this->form_validation->name('name of variable from REQUEST');

#Pattern
$this->form_validation->pattern('pattern');
#see Form_validation class for the list of pattern / eg. url, tel, etc

#Custom pattern
$this->form_validation->custom_pattern('custom pattern');

#Required / Check if field is required
$this->form_validation->required();

#Matches / Check if current field match the other field
$this->form_validation->matches($field)

#Minimum length
$this->form_validation->min_length($min)

#Maximum length
$this->form_validation->max_length($max)

#Valid Email
$this->form_validation->valid_email($email)

#Alpha
$this->form_validation->alpha($field)

#Alpha Numeric
$this->form_validation->alpha_numeric($field)

#Alpha numeric and space
$this->form_validation->alpha_numeric_space($field)

#Alpha and Space
$this->form_validation->alpha_space($field)

#Alpha numeric and dash
$this->form_validation->alpha_numeric_dash($field)

#Numeric
$this->form_validation->numeric($field)

#Integer
$this->form_validation->integer($field)

#Decimal
$this->form_validation->decimal($field)

#Greater than
$this->form_validation->greater_than($field)

#Greater than Equal to
$this->form_validation->greater_than_equal_to($field)

#Less than
$this->form_validation->less_than($field)

#Less than Equal to
$this->form_validation->less_than_equal_to($field)

#In list
$this->form_validation->in_list($field)

#Run
$this->form_validation->run()
#validate and run if no errors
```

### URL Helper

```php
#Redirect
redirect($uri, $method, $sec)
#it has 3 parameters which one of them is required(uri)
#method: Refresh of Location.
#sec: if refresh, secs to redirect

#Loading of javascript files
load_js(array('js1', 'js2'))
#js1/js2 is the filename or path of javascript

#Loading of css files
load_css(array('css1', 'css2'))
#css1/css2 is the filename or path of css

#Create links
site_url('controller/method/args/args')
```

### Form Helper
```php
#Open HTML form
echo form_open('email/send');
#Would produce: <form method="post" accept-charset="utf-8" action="http://example.com/index.php/email/send">
#Adding attributes
$attributes = array('class' => 'email', 'id' => 'myform');
echo form_open('email/send', $attributes);
#or
echo form_open('email/send', 'class="email" id="myform"');
#Hidden fields can be added by passing an associative array to the third parameter, like this:
$hidden = array('username' => 'Joe', 'member_id' => '234');
echo form_open('email/send', '', $hidden);
#You can skip the second parameter by passing any falsy value to it.

#Multipart / This function is absolutely identical to form_open() above, except that it adds a multipart attribute, which is necessary if you would like to use the form to upload files with.
form_open_multipart()

#Hidden input
form_hidden('username', 'johndoe');
#Would produce: <input type="hidden" name="username" value="johndoe" />

$data = array(
        'name'  => 'John Doe',
        'email' => 'john@example.com',
        'url'   => 'http://example.com'
);
echo form_hidden($data);
#Would produce:
#<input type="hidden" name="name" value="John Doe" />
#<input type="hidden" name="email" value="john@example.com" />
#<input type="hidden" name="url" value="http://example.com" />

$data = array(
        'name'  => 'John Doe',
        'email' => 'john@example.com',
        'url'   => 'http://example.com'
);
echo form_hidden('my_array', $data);
#Would produce:
#<input type="hidden" name="my_array[name]" value="John Doe" />
#<input type="hidden" name="my_array[email]" value="john@example.com" />
#<input type="hidden" name="my_array[url]" value="http://example.com" />

$data = array(
        'type'  => 'hidden',
        'name'  => 'email',
        'id'    => 'hiddenemail',
        'value' => 'john@example.com',
        'class' => 'hiddenemail'
);
echo form_input($data);
#Would produce:
#<input type="hidden" name="email" value="john@example.com" id="hiddenemail" class="hiddenemail" />

echo form_input('username', 'johndoe');
$data = array(
        'name'          => 'username',
        'id'            => 'username',
        'value'         => 'johndoe',
        'maxlength'     => '100',
        'size'          => '50',
        'style'         => 'width:50%'
);

echo form_input($data);
#Would produce:
#<input type="text" name="username" value="johndoe" id="username" maxlength="100" size="50" style="width:50%"  />

$js = 'onClick="some_function()"';
echo form_input('username', 'johndoe', $js);
#or
$js = array('onClick' => 'some_function();');
echo form_input('username', 'johndoe', $js);

form_password()
#This function is identical in all respects to the form_input() function above except that it uses the “password” input type.

form_upload()
#This function is identical in all respects to the form_input() function above except that it uses the “file” input type, allowing it to be used to upload files.

form_textarea()
#This function is identical in all respects to the form_input() function above except that it generates a “textarea” type.

form_dropdown()
$options = array(
        'small'         => 'Small Shirt',
        'med'           => 'Medium Shirt',
        'large'         => 'Large Shirt',
        'xlarge'        => 'Extra Large Shirt',
);

$shirts_on_sale = array('small', 'large');
echo form_dropdown('shirts', $options, 'large');
#Would produce:
#<select name="shirts">
#<option value="small">Small Shirt</option>
#<option value="med">Medium  Shirt</option>
#<option value="large" selected="selected">Large Shirt</option>
#<option value="xlarge">Extra Large Shirt</option>
#</select>

echo form_dropdown('shirts', $options, $shirts_on_sale);
#Would produce:
#<select name="shirts" multiple="multiple">
#<option value="small" selected="selected">Small Shirt</option>
#<option value="med">Medium  Shirt</option>
#<option value="large" selected="selected">Large Shirt</option>
#<option value="xlarge">Extra Large Shirt</option>
#</select>

$js = 'id="shirts" onChange="some_function();"';
echo form_dropdown('shirts', $options, 'large', $js);
#or
$js = array(
        'id'       => 'shirts',
        'onChange' => 'some_function();'
);
echo form_dropdown('shirts', $options, 'large', $js);

form_multiselect()
#Lets you create a standard multiselect field. The first parameter will contain the name of the field, the second parameter will contain an associative array of options, and the third parameter will contain the value or values you wish to be selected.

#The parameter usage is identical to using form_dropdown() above, except of course that the name of the field will need to use POST array syntax, e.g. foo[].

form_fieldset()
echo form_fieldset('Address Information');
echo "<p>fieldset content here</p>\n";
echo form_fieldset_close();
#Produces:
#<fieldset>
#<legend>Address Information</legend>
#<p>fieldset content here</p>
#</fieldset>

form_fieldset_close()
$string = '</div></div>';
echo form_fieldset_close($string);
// Would produce: </fieldset></div></div>
```

[Youtube Channel](https://youtube.com/ronmarasigan)

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


