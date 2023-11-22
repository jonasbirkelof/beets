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