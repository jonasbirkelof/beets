Beets PHP contains several tools for you to easily work with your database. On this page you will learn how to setup and connect to the tadabase ase well as basic CRUD functionallity. Beets supports the use of multiple databases.

## Database.php

```php title="Location"
~/App/Core/Database.php
```

```php title="Namespace"
namespace App\Core;
```

```php title="Import"
use App\Core\Database as DB;
```

## Configure the connection

To be able to connect to the datbase you will have to add your credentials to the [.env](./configuration/env.md) file. You will get this information from your hosting service. 

If you use the database that is provided with Beets, you find the default values below. Make sure to change the root password to a strong one.

```php title="~/.env"
DB_CONNECTION=mysql
DB_HOST=mysql_db
DB_PORT=3306
DB_DATABASE=beetsphp
DB_CHARSET=utf8mb4
DB_USERNAME=root
DB_PASSWORD=root
```

The table names can be accessed by constants defined in [~/config/config.php](../configuration/config.md).

```php
define('DB_USER_ACCOUNTS', DB_TABLE_PREFIX . 'admin__user_accounts');
define('DB_ROLES', DB_TABLE_PREFIX . 'admin__roles');
define('DB_PERMISSIONS', DB_TABLE_PREFIX . 'admin__permissions');
define('DB_PERMISSIONS_REL', DB_TABLE_PREFIX . 'admin__permissions_relations');
```

If you need to have a prefix for your tables, you can set it with the `DB_TABLE_PREFIX` constant. The constant can be changed depending on the [application environment](./configuration/env.md#environment):

```php
// Development variables
if (APP_ENV == 'development') {
	$dbTablePrefix = '';
}

// Production variables
if (APP_ENV == 'production') {
	$dbTablePrefix = '';
}

define('DB_TABLE_PREFIX', $dbTablePrefix);
define('DB_USER_ACCOUNTS', DB_TABLE_PREFIX . 'admin__user_accounts');
```

### Additional databases

If you want to use an additional database for fetching data you store someplace else, uncomment the additional credential values in the `.env` file. You will have to change the credentials as well as the host IP/URL and port. 

You can change the database prefix from `DB2` to anything you like, i.e. `REMOTE`: `REMOTE_HOST`.

```php title="~/.env"
REMOTE_CONNECTION=mysql
REMOTE_HOST=10.20.30.40
REMOTE_PORT=3307
REMOTE_DATABASE=dbname_2
REMOTE_CHARSET=utf8mb4
REMOTE_USERNAME=username_2
REMOTE_PASSWORD=password_2
```

Use the [`connection()`](#connection) method for selecting what database to use.

### Default database

The database with prefix `DB` will be used by default. This means that you do not need to specify the connection when fetching the data. You can change the default database by changing the `DEFAULT_DB` value to the prefix of the preferred default database.

```php title="~/.env"
DEFAULT_DB=DB
```

## Connect to the database

By importing Database.php with `use App\Core\Database` you get access to all database functionality. The connection is initiated in the `__construct()` which retrieves the credentials from the [.env](./configuration/env.md) file and uses them to set up a new PDO object.

```php
use App\Core\Database as DB;

DB::query($sql)->fetchAll();
```

## Escape special characters

Before inserting information to your database, you must escape (remove) all special characters so that no harmful code can be submitted. Use the `escape()` function to do so:

```php
$inputName = escape($formData['input_name']);
```

## Database methods

Beets PHP comes with a couple of handy tools for the most commonly used operations but feel free to add your own methods that fits your needs.

### connection()

If you want to use the default database as specified in the `.env` file, you do not need to use the `connection()` method!

To use an alternative database, pass the prefix of that connection as an argument of the `connection()` method.

```php
$sql = "SELECT * FROM users";
$result = DB::connection('REMOTE')->query($sql)->fetchAll();
```

### query()

The `query()` method will combine your SQL query with the attributes for your prepared statements and then execute the operation.

```php
$sql = "SELECT * FROM users";
$result = DB::query($sql)->fetchAll();

$sql = "INSERT INTO users (first_name, last_name) VALUES (?, ?)"
$result = DB::query($sql, ['Jim', 'Halpert']);

$sql = "INSERT INTO users (first_name, last_name) VALUES (:first, :last)"
$result = DB::query($sql, ['first' => 'Jim', 'last' => 'Halpert']);
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

$sql = "SELECT * FROM table";
$result = DB::query($sql)->fetchAll();
```

Here we will get the names of all users in the users table with the status of 1 and order them by their first name. The query will use the PDO method `fetchAll()` to genereate an array of results.

```php title="UserController.php"
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

```php title="User.php"
namespace App\Models;

use App\Core\Database as DB;

class User
{
	public static function get(array $args = []): array
	{
		// Build the where and order clauses
		$orderClause = DB::orderBy(! empty($args['orderBy']) ? $args['orderBy'] : []);
		$whereClause = DB::where(! empty($args['where']) ? $args['where'] : []);

		$sql = "SELECT first_name, last_name FROM users $whereClause $orderClause";
		// Execute the query and store the result in $result
		$result = DB::query($sql)->fetchAll();

		return $result ?: [];
	}
}
```

```php title="index.php"
// print the array with the users data
print_r($users);
```

### Get single post from db

```php
use App\Core\Database as DB;

$sql = "SELECT * FROM table WHERE id = ?";
$params = [$userId];
$result = DB::query($sql, $params)->fetch();
```

Here we will get the names of all users in the users table with the status of 1 and order them by their first name. The query will use the PDO method `fetch()` to get a single item as an array.

We will use a User method called `findOrFail()` that will return a 404 error page if the database doesn't return a result, like if a faulty user id was put in the URL. If you do not want to abort if there is no result, you can just use the `find()` method.

```php title="UserController.php"
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

```php title="User.php"
namespace App\Models;

use App\Core\App;
use App\Core\Database as DB;

class User
{
	public static function find(int $id): array
	{
		$sql = "SELECT first_name, last_name FROM users WHERE id = :id";		
		// Execute and store the result in $result
		$result = DB::query($sql, ['id' => $id])->fetch();

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

```php title="show.php"
// print the array with the user data
print_r($user);
```

### Add post to db

```php
use App\Core\Databas as DB;

$sql = "INSERT INTO table (col_1, col_2) VALUES (?, ?)";
$result = DB::query($sql, ['foo', 'bar']);
```

Here we will add a new user to the users table. The values for `firstName` and `lastName` is assumed to come from a form and as retrieved from the `$_POST` super global. 

Please note that we are not covering the validation functionality in this example. The validation errors and input values are stored as flash messages to be used in case of errors to show what part of the validation failed.

```php title="UserController.php"
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

```php title="User.php"
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
			$sql = "INSERT INTO users (first_name, last_name) VALUES (:firstName, :lastName)";
			// Execute
			DB::query($sql, [
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

$sql = "UPDATE table SET col_1 = ?, col_2 = ? WHERE id = ?";
$result = DB::query($sql, ['foo', 'bar', 123]);
```

Here we will update the user with id of `$userId` in the users table. The values for `firstName` and `lastName` is assumed to come from a form and as retrieved from the `$_POST` super global. The `$userId` comes from the router file.

Please note that we are not covering the validation functionality in this example. The validation errors and input values are stored as flash messages to be used in case of errors to show what part of the validation failed.

```php title="UserController.php"
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

```php title="User.php"
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
			$sql = "UPDATE users  SET first_name = :firstName, last_name = :lastName WHERE id = :id";
			// Execute
			DB::query($sql, [
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

$sql = "DELETE FROM table WHERE id = ?";
$result = DB::query($sql, [123]);
```

Here we will delete the user with id of `$userId` from the users table. The `$userId` comes from the router file.

```php title="UserController.php"
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

```php title="User.php"
namespace App\Models;

use App\Core\Database as DB;

class User
{
	public static function destroy(int $userId): bool
	{
		$sql = "DELETE FROM " . static::DB_TABLE . " WHERE id = :id";
		// Execute
		DB::query($sql, ['id' => $userId]);

		return true;
	}
}
```