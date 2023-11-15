# Database

Beets PHP contains several tools for you to easily work with your database. On this page you will learn how to setup and connect to the tadabase ase well as basic CRUD functionallity.

## Configure the connection

To be able to connect to the datbase you will have to add your credentials to the [.env](./configuration/env.md) file. You will get this information from your hosting service. 

If you are using the local database you can set `DB_DATABASE=` to your database name, `DB_USENAME=root` and `DB_PASSWORD=` to be empty like the example below.

```php title="~/.env"
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=dbname
DB_CHARSET=utf8mb4
DB_USERNAME=username
DB_PASSWORD=
```

## Connect to the database

By importing [`Database.php`](./classes/Database.md) with `use App\Core\Database` you get access to all database functionality. The connection is initiated in the `__construct()` which retrieves the credentials from the [.env](./configuration/env.md) file and uses them to set up a new PDO object.

```php
use App\Core\Database as DB;

$db = new DB();
```

## Database methods

Beets PHP comes with a couple of handy tools for the most commonly used operations but feel free to add your own methods that fits your needs.

### query()

The `query()` method will combine your SQL query with the attributes for your prepared statements and then execute the operation.

```php
$sql = "SELECT * FROM users";
$result = $db->query($sql);

$sql = "INSERT INTO users (first_name, last_name) VALUES (?, ?)"
$result = $db->query($sql, ['Jim', 'Halpert']);

$sql = "INSERT INTO users (first_name, last_name) VALUES (:first, :last)"
$result = $db->query($sql, ['first' => 'Jim', 'last' => 'Halpert']);
```

### where()

The `where()` method will create a string that can be used in your SQL query to build your simple filter. The arguments are passed as an array.

The default string will be passed if no arguments are provided. This string is supposed to show every row in the table.

```php
$where = DB::where(['status = 1']);
$where = DB::where(['status = 1', 'AND category = 2']);

$where = DB::where();
```

Output:

```
WHERE status = 1
WHERE status = 1 AND category = 2

WHERE id IS NOT NULL
```

### orderBy()

The `orderBy()` method will create a string that can be used in your SQL query to build your simple filter. The arguments are passed as an array.

The default string will be passed if no arguments are provided. This string is supposed to sort the results by oldest first.

```php
$where = DB::orderBy(['first_name ASC']); // WHERE column_a = 1
$where = DB::orderBy(['first_name ASC', 'id ASC']); // WHERE column_a = 1 AND column_b = 2

$where = DB::orderBy();
```

Output:

```
ORDER BY first_name ASC
ORDER BY first_name ASC, id ASC

ORDER BY id ASC
```

## Usage

Here you can find some common use cases for the database in a CRUD application. In the examples below we assume that you are using a RESTful approach with the MVC file system that comes with Beets PHP.

We will use the controller `UserController.php` to call the model `User.php` to get, insert, update or delete data.

It is by using this approach the `where()` and `orderBy()` methods comes in handy!

### Get multiple posts from db

```php
use App\Core\Database as DB;

$db = new App\Core\Database();
$result = $db->query("SELECT * FROM table")->fetchAll();
```

Here we will get the names of all users in the users table with the status of 1 and order them by their first name. The query will use the PDO method `fetchAll()` to genereate an array of results.

```php title="~/app/http/controllers/UserController.php"
namespace App\Http\Controllers;

use App\Core\App;
use App\Models\User;

class UserController
{
	public static function index()
	{
		// Get the users data
		$users = User::get(
			'where' => ['status = 1'],
			'orderBy' => ['first_name ASC'],
		);

		// Return the view with the users data
		return App::view('/users/index.php', [
			'users' => $users
		]);
	}
}
```

```php title="~/app/models/User.php"
namespace App\Models;

use App\Core\Database as DB;

class User
{
	public static function get(array $args = []): array
	{
		// Build the where and order clauses
		$orderClause = DB::orderBy(! empty($args['orderBy']) ? $args['orderBy'] : []);
		$whereClause = DB::where(! empty($args['where']) ? $args['where'] : []);

		$db = new DB();
		$sql = "SELECT first_name, last_name FROM users $whereClause $orderClause";
		
		// Execute the query and store the result in $result
		$result = $db->query($sql)->fetchAll();

		return $result ?: [];
	}
}
```

```php title="~/public/views/users/index.php"
// print the array with the users data
print_r($users);
```

### Get single post from db

```php
use App\Core\Database as DB;

$db = new App\Core\Database();
$result = $db->query("SELECT * FROM table")->fetch();
```

Here we will get the names of all users in the users table with the status of 1 and order them by their first name. The query will use the PDO method `fetch()` to get a single item as an array.

We will use a User method called `findOrFail()` that will return a 404 error page if the database doesn't return a result, like if a faulty user id was put in the URL. If you do not want to abort if there is no result, you can just use the `find()` method.

```php title="~/app/http/controllers/UserController.php"
namespace App\Http\Controllers;

use App\Core\App;
use App\Models\User;

class UserController
{
	public static function show(int $userId)
	{
		// Get the user data
		$user = User::findOrFail($userId);

		// Return the view with the user data
		return App::view('/users/show.php', [
			'user' => $user
		]);
	}
}
```

```php title="~/app/models/User.php"
namespace App\Models;

use App\Core\App;
use App\Core\Database as DB;

class User
{
	public static function find(int $id): array
	{
		$db = new DB();
		$sql = "SELECT first_name, last_name FROM users WHERE id = :id";

		// Execute the query and store the result in $result
		$result = $db->query($sql, ['id' => $id])->fetch();

		return $result ?: [];
	}

	public static function findOrFail(int $id): mixed
	{
		$result = static::find($id);

		if (! $result) {
			App::abort();
		}

		return $result;
	}
}
```

```php title="~/public/views/users/show.php"
// print the array with the user data
print_r($users);
```

### Add post to db

```php
use App\Core\Databas as DB;

$db = new DB();
$sql = "INSERT INTO table (col_1, col_2) VALUES (?, ?)";
$result = $db->query($sql, ['foo', 'bar']);
```

Here we will add a new user to the users table. The values for `firstName` and `lastName` is assumed to come from a form and as retrieved from the `$_POST` super global. 

Please note that we are not covering the validation functionality in this example. The validation errors and input values are stored as flash messages to be used in case of errors to show what part of the validation failed.

```php title="~/app/http/controllers/UserController.php"
namespace App\Http\Controllers;

use App\Core\Redirect;
use App\Models\User;

class UserController
{
	public static function store()
	{
		// Try to store the user in the database.
		// If not successfull, redirect back to the form
		// and show an error message.
		if (! User::store($_POST)) {
			Redirect::to("/users/create")->with("message", "Error");
		}
		
		// If the store was successful, redirect
		Redirect::to("/users")->with("message", "Success");
	}
}
```

```php title="~/app/models/User.php"
namespace App\Models;

use App\Http\Form;
use App\Core\Database as DB;
use App\Core\Session;

class User
{
	public static function store(array $formData): bool
	{
		// Collect the input data
		$firstName = escape($formData['first_name']);
		$lastName = escape($formData['last_name']);

		// Validate the data
		$Form = new Form();		
		$Form->validate('first_name', $firstName)->name();
		$Form->validate('last_name', $lastName)->name();

		// Store the data if there are no errors
		if (! $Form->errors()) {
			$db = new DB();
			$sql = "INSERT INTO users (first_name, last_name) VALUES (:firstName, :lastName)";

			// Execute the query
			$db->query($sql, [
				'firstName' => $firstName,
				'lastName' => $lastName
			]);
		}

		// Flash validation errors and input values
		Session::flash('errors', $Form->errors());
		Session::flash('old', [
			'first_name' => $firstName,
			'last_name' => $lastName
		]);

		// Return true (no errors) or false (has errors)
		return empty($Form->errors());
	}
}
```

### Update post in db

```php
use App\Core\Databas as DB;

$db = new DB();
$sql = "UPDATE table SET col_1 = ?, col_2 = ? WHERE id = ?";
$result = $db->query($sql, ['foo', 'bar', 123]);
```

Here we will update the user with id of `$userId` in the users table. The values for `firstName` and `lastName` is assumed to come from a form and as retrieved from the `$_POST` super global. The `$userId` comes from the router file.

Please note that we are not covering the validation functionality in this example. The validation errors and input values are stored as flash messages to be used in case of errors to show what part of the validation failed.

```php title="~/app/http/controllers/UserController.php"
namespace App\Http\Controllers;

use App\Core\Redirect;
use App\Models\User;

class UserController
{
	public static function update(int $userId)
	{
		// Try to update the user in the database.
		// If not successfull, redirect back to the form
		// and show an error message.
		if (! User::update($_POST, $userId)) {
			Redirect::to("/users/$userId/edit")->with("message", "Error");
		}
		
		// If the store was successful, redirect
		Redirect::to("/users/$userId")->with("message", "Success");
	}
}
```

```php title="~/app/models/User.php"
namespace App\Models;

use App\Http\Form;
use App\Core\Database as DB;
use App\Core\Session;

class User
{
	public static function update(array $formData, int $userId): bool
	{
		// Collect the input data
		$firstName = escape($formData['first_name']);
		$lastName = escape($formData['last_name']);

		// Validate the data
		$Form = new Form();		
		$Form->validate('first_name', $firstName)->name();
		$Form->validate('last_name', $lastName)->name();

		// Store the data if there are no errors
		if (! $Form->errors()) {
			$db = new DB();
			$sql = "UPDATE users  SET first_name = :firstName, last_name = :lastName WHERE id = :id";

			// Execute the query
			$db->query($sql, [
				'firstName' => $firstName,
				'lastName' => $lastName,
				'id' => $userId
			]);
		}

		// Flash validation errors and input values
		Session::flash('errors', $Form->errors());
		Session::flash('old', [
			'first_name' => $firstName,
			'last_name' => $lastName
		]);

		// Return true (no errors) or false (has errors)
		return empty($Form->errors());
	}
}
```

### Delete post from db

```php
use App\Core\Databas as DB;

$db = new DB();
$sql = "DELETE FROM table WHERE id = ?";
$result = $db->query($sql, [123]);
```

Here we will delete the user with id of `$userId` from the users table. The `$userId` comes from the router file.

```php title="~/app/http/controllers/UserController.php"
namespace App\Http\Controllers;

use App\Core\Redirect;
use App\Models\User;

class UserController
{
	public static function destroy(int $userId)
	{
		// Delete the user
		User::destroy($userId);

		// Redirect
		Redirect::to("/users")->with("message", "Success");
	}
}
```

```php title="~/app/models/User.php"
namespace App\Models;

use App\Core\Database as DB;

class User
{
	public static function destroy(int $userId): bool
	{
		$db = new DB();
		$sql = "DELETE FROM " . static::DB_TABLE . " WHERE id = :id";

		// Execute the query
		$db->query($sql, ['id' => $userId]);

		return true;
	}
}
```