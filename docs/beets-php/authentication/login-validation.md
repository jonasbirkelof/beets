To see if a user has logged in or not, we have a couple of handy methods. They check wether a user session has been created.

```php title="Example"
use App\Core\Authenticate as Auth;

if (Auth::check()) {
	echo "Logged in";
}

if (Auth::guest()) {
	echo "Not logged in";
}
```

There are also helper functions that might be more convenient to use, especially within the views.

```php
if (auth()) {
	echo "Logged in";
}

if (guest()) {
	echo "Not logged in";
}
```

```html
<?php if (auth()) : ?>
<p>This text is shown when logged in"</p>
<?php endif; ?>
```

The helper functions are located in `~/app/core/includes/auth.php`.