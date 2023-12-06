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

Redirect::to("/users");
```

This example will redirect the visitor to the `/users` page.

## Redirect with a flash message

If you cascade the `to()` method with the `with()` method you can pass along a flash message. This is convenient way to make your code more compact and easy to read. The `with()` methoid basically just uses the `Session::flash()`method so you [use it](./sessions.md/#set-a-flash-message) in the same way.

The Session class has been included within the Redirect class due to the dependency so you do not need to include it again to just set the flash message.

```php
use App\Core\Redirect;

Redirect::to("/users")->with("message", "The user was saved!");
```

To catch the message you [get it](./sessions.md#get-the-flash-message) like normal:

```php title=".../users"
use App\Core\Session;

echo Session::get("message"); // The user was saved!
```

## Save and use request URI

If you are redirecting traffic as a result of a validation failure you can svare the request URI (the path you want to visit) in the session with the `saveRequestUri()` method so that you can use it later with `useRequestUti()`. 

An example of this is if users tries to access a page that can only be accessed when logged in. If they are not logged in they can be redirected to the login page and when logged in be redirected to the page that they were trying to visit before.

Below we save the requested URI with `saveRequestUri()` when we redirect to the login page when the authentication fails in the router.

```php title="web.php" hl_lines="3"
$Router->mount('/users', function () use ($Router) {
	$Router->before('GET', '*(.*)', function () {
		if (! Auth::check()) Redirect::to('/login')->saveRequestUri();
	});

	$Router->get('/', [UserController::class, 'index']);
	...
});
```

When the login form is submitted after the redirect we can append `useRequestUri()` to use the URI stored in the session (if there is one) instead of the one submitted in `to()`.

```php title="LoginController.php" hl_lines="12"
use App\Core\Redirect;

class LoginController
{
	public static function store()
	{
		// Attempt to log in the user with submitted credentials

		// Store user information in session

		// Redirect to "/" if no request URI is set
		Redirect::to("/")->useRequestUri();
	}
}
```

When the redirect with `useRedirectUri()` is executed, the session will remove the variable automatically.