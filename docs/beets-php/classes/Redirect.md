# Redirect.php

This class contains methods for redirecting a user with the possibility to pass along a flash message using the [Session class](./Session.md).

## Location

```php
~/app/core/Redirect.php
```

## Namespace

```php
namespace App\Core;
```

## Import the class

```php
use App\Core\Redirect; // Import the Redirect class
```

## Dependencies

```php
use App\Core\Session; // Import the Session class
```

## Properties

```php
private static $targetPath;
```

## Methods

### to()

```php
public static function to(string $targetPath): object
{
	static::$targetPath = $targetPath;
	return new static();
}
```

### with()

```php
public function with($key, $value): object
{
	Session::flash($key, $value);
	return $this;
}
```

## Destructor

```php
public function __destruct()
{
	header("location: " . static::$targetPath);
	exit();
}
```