# Database.php

This class contains methods for accessing data in your database. See the [database page](../database.md) for instructions on how to connect to the database and use this class.

## Location

```php
~/app/core/Database.php
```

## Namespace

```php
namespace App\Core;
```

## Import the class

```php
use App\Core\Database; // Import the Database class
```

## Dependencies

```php
use PDO; // Import the PDO object
```

## Properties

### $connection

```php
public $connection;
```

### $order

```php
public static $order;
```

## Methods

### query()

```php
public static function query(string $query, array $params = [])
{
	$stmt = (new static)->connection->prepare($query);
	$stmt->execute($params);

	return $stmt;
}
```

### orderBy()

```php
public static function orderBy($args, $defaultClause = "ORDER BY id ASC") : string
{
	if (empty($args)) {
		return $defaultClause;
	}

	$clause = "ORDER BY";

	foreach ($args as $key => $column) {
		$clause .= ' ' . htmlentities($column);
		$clause .= $key !== array_key_last($args) ? ',' : '';
	}

	return $clause;
}
```

### where()

```php
public static function where($args, $defaultClause = "WHERE id IS NOT NULL") : string
{
	if (empty($args)) {
		return $defaultClause;
	}

	$clause = "WHERE";

	foreach ($args as $key => $column) {
		$clause .= ' ' . htmlentities($column);
	}

	return $clause;		
}
```

## Constructor

```php
public function __construct()
{
	// Collect credentials from .env
	$dbConnection = $_ENV['DB_CONNECTION'];
	$dbUsername = $_ENV['DB_USERNAME'];
	$dbPassword = $_ENV['DB_PASSWORD'];
	$dbConfig = [
		'host' => $_ENV['DB_HOST'],
		'port' => $_ENV['DB_PORT'],
		'dbname' => $_ENV['DB_DATABASE'],
		'charset' => $_ENV['DB_CHARSET']
	];

	// Build PDO DSN string using the $config array
	$dsn = $dbConnection . ":" . http_build_query($dbConfig, '', ';');

	// Assign a new PDO instance to the $connection variable
	$this->connection = new PDO($dsn, $dbUsername, $dbPassword, [
		PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
	]);
}
```