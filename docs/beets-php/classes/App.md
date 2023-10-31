# App.php

```php
use: App\Core\App; // Import the App class
```

The `App` class contains basic methods that can be used throughout the application.


## view()

```php
public static function view($pattern, $attributes = [])
{
	extract($attributes);

	require_once PUBLIC_ROOT . 'views/' . $pattern;
}
```

The view function is used by the router to load the requested views.

**Example**

```php title="./app/http/controllers/UserController.php"
namespace App\Http\Controllers;

use App\Core\App;

class UserController
{
	public static function index()
	{		
		return App::view('users/index.php', [
			'title' => 'User Accounts',
			'list' => ['item 1', 'item 2', 'item 3']
		]);
	}
}
```

In this example, when the router is requesting the index page from the user controller, the controller returns the view using the `view()` method. Apart from the http pattern (the url), additional attributes can be passed to be recieved in the loaded page like this:

```php title="./public/views/users/index.php"
echo $title; // User Accounts
echo $list[0]; // item 1
```

## message()

The `message()` method provides a way to generate massages suitable for testing or troubleshooting. THe messages can also be used to clearly show an erroneous input.

```php
public static function message($message, $level = 'default')
{
	...
}
```

There are five message levels to choose from: `error`, `warning`, `success`, `info` and `default`.

**Example**

```php
use App\Core\App;

echo App::message("Default message");
echo App::message("Error message", "error");
echo App::message("Warning message", "warning");
echo App::message("Success message", "success");
echo App::message("Info message", "info");
```

The example above generates:

- <span style="background-color: white; color: black;">MESSAGE: Default message</span>
- <span style="background-color: red; color: white;">ERROR: Error message</span>
- <span style="background-color: orange; color: black;">WARNING: Warning message</span>
- <span style="background-color: green; color: white;">SUCCESS: Success message</span>
- <span style="background-color: blue; color: white;">INFO: Info message</span>

## abort()

```php
public static function abort($code = 400) {
	http_response_code($code);
	require PUBLIC_ROOT . "views/errors/{$code}.php";
	die();
}
```

By calling the abort method, the code will generate a response code (404 by default), show the abort page and the abort the code.

**Example**

```php
use App\Core\App;

if (! $condition) {
	App::abort();
}
```

## error()

```php
use App\Core\Session;

public static function error($message = null) {
	Session::flash('message', $message);
		
	http_response_code(200);
	require PUBLIC_ROOT . "views/errors/error.php";
	die();
}
```

Similar to the [`abort()`](#abort) method, the `error()` method will abort the code after setting the reponse code to 200 and loading the error page. The error page can be injected with an error message (string) that is null by default.

Note that this method is dependent on the `Session` class since it uses the `#!php Session::flash()` method to set the message.

**Example**

```php
use App\Core\App;

if (! $condition) {
	App::error("The condition was not fulfilled!");
}
```