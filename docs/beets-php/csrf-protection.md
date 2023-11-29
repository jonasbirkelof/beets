# CSRF Protection

With Beets PHP you get a simple CSRF (Cross Site Request Forgery) protection class. This class is yet pretty basic, so you might want to take extra security measurements, but it's a start nevertheless!

## CSRF.php

```php title="Location"
~/app/core/CSRF.php
```

```php title="Namespace"
namespace App\Core;
```

```php title="Import"
use App\Core\CSRF;
```

## How to use the CSRF protection

The Beets CSRF protection will create a new token when a page with a form loads. This can be done in the controller before the view is returned. We can then add a hidden form field using the [`csrf()`](#the-csrf-form-helper-function) helper function. When the form is submitted we can check the submitted value against what we are expecting it to be (the token that we created when we loaded the page).

## Create a new token

To create a new CSRF token we run:

```php
use App\Core\CSRF;

CSRF::newToken();
```

The new token is then saved to the session (`#!php $_SESSION['_csrf']`) and hashed with a secret key that is known only to the server. The hashed version of the token is then stored as a private property inside the CSRF object. This property is used later when we want to do the [validation](#validate-a-submitted-token).

## The csrf() form helper function

The `csrf()` helper function is used to populate a `<form>` tag with the field that passes along the CSRF token.

```html
<form method="POST" name="my-form" id="myForm">
	<?= csrf() ?>
	...
</form>
```

Output:

```html
<form method="POST" name="my-form" id="myForm">
	<input type="hidden" name="_csrf" value="545001ecfe2a9487bdb3a30534ebcaf9fa72456305863cb2aa3a09afaa6ce524">
	...
</form>
```

## Validate a submitted token

To validate the form after submition we use the `CSRF::validate()` method. The method will grab the `#!php $_POST['_csrf']` field value and compare it with the expected hashed value. The expected value is created by hashing the token that was created for the form when it was loaded with the secret key that is only known to the server.

The method will return a boolean of `true` if the tokens match and `false` if they don't. Use this in the controller to either show an error page or redirect the user to somewhare.

!!! warning "You should log any attempt to access a page with an invalid CSRF token!"

```php
// Update post
public static function update(int $postId)
{
	CSRF::validate();
	
	// Update post
	
	// Redirect
}
```

The code will only be aborted if the validation fails.