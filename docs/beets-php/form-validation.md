Beets PHP has a class with methods for form validation. Access them by importing the [`Form.php`](./classes/Form.md) class.

## How validation works

You can validate submitted form data before updating your database. If the validation fails, an error message is stored in an array. 

You can check if the array contains values and in that case prevent the database query to execute and instead return to the form and show the error messages. The process can look something like this:

```php
use App\Core\Database as DB;
use App\Core\Redirect;
use App\Core\Session;
use App\Http\Form;

// retrieve and sanitize the inputs
$name = escape($_POST['name']);

// Validate the inputs
$Form = new Form();
$Form->validate('input_name', $name)->name()->required();

// Check for errors
if (! $Form->errors()) {
	$sql = "INSERT INTO table (name) VALUES (?)";
	$result = DB::query($sql, [$name]);

	// Redirect
	Redirect::to("path/to/next/page");
} else {
	// Use session flash to store errors and old values
	Session::flash('errors', $Form->errors());
	Session::flash('old', ['name' => $name]);

	// Redirect back to form
	Redirect::to("/path/to/form");
}
```

## Validation methods

### Initiate validation

```php
public function validate($field, $value): object
{
	...
}
```

The `validate()` method is used to set the `$field` and `$value` properties. 

- `$field` is used when generating an error message so that you can assign the correct error message to the corresponding form input. 
- `$value` is stored so that the cascading methods like `required()`, `name()` and others can use it to validate it.

```html
<form>
	<input type="text" name="input_name" value="">
</form>
```
```php
$value = escape($_POST['input_name']); // ""

$Form = new Form();
$Form->validate('input_name', $value)->required();
```

### Filled field

```php
public function isFilled(): bool
{
	...
}
```

Check if an input has been filled out. This method takes no arguments and returns a boolean.

The method checks if the value is **empty ("")**, **null** or the **lenght is 0** and returns `false` if any. Blank spaces does not count as characters. An input containing only blank spaces is considered empty.

While you CAN use this method, it's important to know that it does not return an error message.

```php
$inputString = "Hello World!";
$inputEmpty = " ";
$inputNull = null;

$Form = new Form();
$Form->validate('input_string', $inputString)->isFilled(); // true
$Form->validate('input_empty', $inputEmpty)->isFilled(); // false
$Form->validate('input_null', $inputNull)->isFilled(); // false
```

### Required field

```php
private const REQUIRED = "The field is required";

public function required($errorMessage = self::REQUIRED): object
{
	...
}
```

The `required()` method uses the `isFilled()` method to check if the input has been filled out correctly. If it's not filled in, an error message is genereated and stored in the `errors` array.

```php
$Form->validate('input_1', $input1)->required();
```

#### Error message

```php
["required"] => "The field is required"
```

You can provide a custom error message:

```php
$Form->validate('input_2', $input2)->required("Custom message");
```

### Unique value in db

```php
private const UNIQUE = "The value already exists";

public function unique(
	$col, 
	$table, 
	$ignoreId = null, 
	$errorMessage = self::UNIQUE
): object
{
	...
}
```

You can check if the value of an input is already stored in the database. A common example is to check if an email address already exists in the database. In that case, the user already has an account on your site.

You add the `unique()` method to your validation and pass the column and table that should be checked in the database.

```php
$Form->validate('email', $email)->unique("email", "users_table");
```

#### Ignore row ID

You have the option to pass a row ID that should be ignored. Let's say the logged in user is updating their personal info. Then their email is already in the database when the validation class checks and would return an error message. If you pass the logged in user ID, the function will just skip that row when checking emails.

```php
$Form->validate('email', $email)->unique("email", "users_table", $userId);
```

#### Error message

```php
["unique"] => "The value already exists"
```

You can provide a custom error message:

```php
$Form->validate('input', $input)->unique("email", "users_table", , "Custom message");
```

### Matching values

```php
private const MATCHING = "The values doesn't match";

public function matching($matchingValue, $errorMessage = self::MATCHING): object
{
	...
}
```

You can check if the `$value` matches another value given as `$matchingValue` in the `matching()` method. If not, an error messagage will be generated.

```php
$pass = "mypassword123";
$pass_conf = "mypass987";

$Form->validate('input_pass', $pass)->matching($pass_conf); // Does not match
```

#### Error message

```php
["matching"] => "The values doesn't match"
```

You can provide a custom error message:

```php
$Form->validate('input_pass', $pass)->matching($pass_conf, "Custom message");
```

### Name

```php

```

### Email

```php

```

### Password

```php

```