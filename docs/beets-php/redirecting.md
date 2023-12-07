# Redirecting

With Beets PHP you get a convenient way to redirect the user to a different page using the Redirect class. This is a simple but effective class.

## Redirect.php

```php title="Location"
~/app/core/Redirect.php
```

```php title="Namespace"
namespace App\Core;
```

```php title="Import"
use App\Core\Redirect;
```

## How to redirect

By calling the redirect class you get access to the `to()` setter that sets the `$targetPath` property to the target path of the redirect. The class destructor then runs the `header()` function using `$targetPath`.

```php
use App\Core\Redirect;

Redirect::to('/users');
```

This example will redirect the visitor to the `/users` page.

## Redirect with a flash message

If you cascade the `to()` method with the `with()` method you can pass along a flash message. This is convenient way to make your code more compact and easy to read. The `with()` methoid basically just uses the `Session::flash()`method so you [use it](./sessions.md/#set-a-flash-message) in the same way.

The Session class has been included within the Redirect class due to the dependency so you do not need to include it again to just set the flash message.

```php
use App\Core\Redirect;

Redirect::to('/users')->with('message', 'The user was saved!');
```

To catch the message you [get it](./sessions.md#get-the-flash-message) like normal:

```php title=".../users"
use App\Core\Session;

echo Session::get('message'); // The user was saved!
```

## Prefer request URI

Every time a redirect is made, the request URI (`#!php $_SERVER['REQUEST_URI']`) is stored in the session.

If you are redirecting traffic as a result of a validation failure, you can use the request URI in the session with `preferRequestUti()` to redirect the user the the page they were trying to visit while logged out. 

An example of this is if users tries to access a page that can only be accessed when logged in. If they are not logged in they can be redirected to the login page and when logged in be redirected to the page that they were trying to visit before.

When the login form is submitted after the redirect we can append `preferRequestUri()` to use the URI stored in the session instead of the default one in `to()`.

```php title="LoginController.php" hl_lines="12"
use App\Core\Redirect;

class LoginController
{
	public static function store()
	{
		// Attempt to log in the user with submitted credentials
		...

		// Store user information in session
		...

		// Redirect to "/" if no request URI is set
		Redirect::to("/")->preferRequestUri();
	}
}
```

The session variable for the request URI will be flushed efter being executed.