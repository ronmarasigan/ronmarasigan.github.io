# LavaLust Framework Documentation

## Database (Query Builder)

## List of Methods

### Raw Query

```php
$this->db->raw('select * from users where username = ?', array($username));
# Output: "SELECT * FROM users WHERE username = ?"
```

### Insert

```php
$bind = array(
	'username' => $username,
	'password' => $this->passwordhash($password),
	'email' => $email,
	'usertype' => $usertype,
	);

$this->db->table('user')->insert($bind);
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

$this->db->table('users')->where('id', 10)->update($data);
# Output: "UPDATE users SET username='ronmarasigan', password='pass', activation='1', status='1' WHERE id='10'"
```

### delete

```php
$this->db->table('table')->where("id", 17)->delete();
# Output: "DELETE FROM table WHERE id = '17'"

# OR

$this->db->table('table')->delete();
# Output: "TRUNCATE TABLE delete"
```

### transaction

```php
$this->db->transaction();

$data = [
	'title' => 'new title',
	'status' => 2
];
$this->db->table('table')->where('id', 10)->update($data);

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
# get_all(): return multiple records.

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
# Output: "SELECT * FROM test limit 10 offset 10";
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
