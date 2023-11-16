# Session.php

This class contains methods for setting, getting and resetting global session variables and flash messages, as well as destroying the session completely.

## Location

```php
~/app/core/Session.php
```

## Namespace

```php
namespace App\Core;
```

## Import the class

```php
use App\Core\Session; // Import the Session class
```

## Dependencies

None.

## Properties

None.

## Methods

### has()

```php
public static function has($key): bool
{
	return (bool) static::get($key);
}
```

### put()

```php
public static function put($key, $value)
{
	$_SESSION[$key] = $value;
}
```

### get()

```php
public static function get($key, $default = null): mixed
{
	return $_SESSION['_flash'][$key] ?? $_SESSION[$key] ?? $default;
}
```

### flash()

```php
public static function flash($key, $value): void
{
	$_SESSION['_flash'][$key] = $value;
}
```

### unflash()

```php
public static function unflash(): void
{
	unset($_SESSION['_flash']);
}
```

### flush()

```php
public static function flush($key = null): void
{
	if ($key) {
		unset($_SESSION[$key]);
	} else {
		$_SESSION = [];
	}
}
```

### destroy()

```php
public static function destroy(): void
{
	static::flush();

	session_destroy();

	$params = session_get_cookie_params();
	setcookie('PHPSESSID', '', time() - 3600, $params['path'], $params['domain'], $params['secure'], $params['httponly']);
}
```