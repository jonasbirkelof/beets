Beets PHP has a class with methods for form validation. Access them by importing the [`Form.php`](./classes/Form.md) class.

## How validation works

You can validate submitted form data before updating your database. If the validation fails, an error message is stored in an array. 

You can check if the array contains values and in that case prevent the database query to execute and instead return to the form and show the error messages. The process can look something like this:

```php title="Example"
use App\Core\Database as DB;
use App\Core\Redirect;
use App\Core\Session;
use App\Http\Form;

// Retrieve and sanitize the inputs
$name = escape($_POST['input_name']);
$email = escape($_POST['input_email']);

// Validate the inputs
$Form = new Form();
$Form->validate('input_name', $name)->name()->required();
$Form->validate('input_email', $email)
	->email([
		'error' => "That's not an email address, that's a pickle!"
	])
	->unique('email', 'users')
	->required();

// Check for errors
if (! $Form->errors()) {
	$sql = "INSERT INTO users (name, email) VALUES (?, ?)";
	$result = DB::query($sql, [$name, $email]);

	Redirect::to('path');
} else {
	// Flash errors and old values
	Session::flash('errors', $Form->errors());
	Session::flash('old', [
		'name' => $name,
		'email' => $email
	]);

	Redirect::to('/back-to-form');
}
```

## Initiate validation

### validate()

The `validate()` method sets the `$field` and `$value` properties that are used with the validation methods as well as storing the value length as `$valueLength`.

```php title="validate()"
public function validate(string $field, $value): object
{
	$this->field = $field;
	$this->value = $value;
	$this->valueLength = mb_strlen($value);

	return $this;
}
```

- `$field` is used when generating an error message so that you can assign the correct error message to the corresponding form input. 
- `$value` is stored so that the cascading methods like `required()`, `name()` and others can use it to validate it.
- `$valueLength` is the length of `$value` ("hello" = 5).

All of these properties are private.

```html title="Example"
<form>
	<input type="text" name="input_field" value="">
</form>
```
```php title="Example"
use App\Http\Form;

$input = escape($_POST['input_field']); // ""

$Form = new Form();

$Form->validate('input_field', $input)->required(); // Fail and generate an error
```

## Error messages

When a validation fails, an error message is created ([`error()`](#error)) and placed in an array ([`errors()`](#errors)) so that the user can get feedback on what data was not submitted correctly.

### errors()

Returns the array that holds the error messages.

```php
public function errors(string $field = null): array
{
	return $field ? $this->errors[$field] : $this->errors;
}
```

```php title="Example"
print_r($Form->errors());

// Result:
array (
	["input_field"] => array(
		["required"] => "The field is required"
	)
)
```
```php title="Example"
print_r($Form->errors('input_field'));

// Result:
array (
	["required"] => "The field is required"
)
```
```php title="Example"
echo $Form->errors('input_field')['required']; 

// Result:
The field is required
```

The array can hold many input field's messages, but each input field can only hold the latest validadion error. 

Example: if you validate a field that can only be alpha characters and max 10 characters long, but is submitted 20 characters long and containing characters like "&!#", tehn only the last check will be stored. If that was the length error, the characters error will be shown if the length is shortened.

### error()

This method puts a new error message into the [`errors()`](#errors) array.

```php
public function error(string $validationLabel, string $field, $message): void
{
	$this->errors[$field] = [$validationLabel => $message];
}
```

- `$validationLabel` is the name of the validation that was performed.
- `$field` is the name of the field that was validated.
- `$message` is the message that should be displayed.

```php title="Example"
public function required()
{
	if (! $this->isFilled()) {
		$this->error('required', $this->field, "The field is required");
	}

	return $this;
}
```
```php title="Example"
$input = escape($_POST['input_field']) // Empty field: ""

$Form = new Form();

$Form->validate('input_field', $input)->required(); // Fail

echo $Form->errors('input_field')['required']; // The field is required
```

## Validation methods

To make a validation you can cascade any validation method (below) to the `validate()` method. 

Most of the methods generate an error message if the validation fails. The error message can be overridden for any method using the paramaters array should you wish to change it. 

Some methods also has other parameters that can be set, such as min and max values. Refer to each method for which parameters are available.

This is an example of how you set the parameters:

```php title="Example"
$Form->validate('input_field', $input)->required([
	'error' => "This is a custom error message"
]);

$Form->validate('input_field', $input)->name([
	'min' => 1,
	'max' => 50
]);

```

### Alpha

Check if `$value` only contains alphabetic characters (a-z, A-Z).

```php
public function alpha(array $params = []): object
{
	...
}
```

```php title="Example"
$Form->validate("input_field", "Darryl")->alpha();

// Pass
```

```title="Default error message"
The field can only contain alphabetic characters (a-z, A-Z)
```

```php title="Parameters"
$Form->validate('input_field', $input)->alpha([
	'error' => "Custom error message"
]);
```

### Alpha Numeric

Check if `$value` ony contains alpha (a-z, A-Z) or numeric (0-9) characters.

```php
public function alphaNumeric(array $params = []): object
{
	...
}
```

```php title="Example"
$Form->validate("input_field", "Nice69")->alphaNumeric();

// Pass
```

```title="Default error message"
The field can only contain alphanumeric characters (a-z, A-Z, 0-9)
```

```php title="Parameters"
[
	'error' => "Custom error message"
]
```

### Email

Check if `$value` is a valid email address.

```php
public function email(array $params = []): object
{
	...
}
```

The submitted email address is filtered and validated using the native PHP `filter_var($email, FILTER_VALIDATE_EMAIL)` function.

```php title="Example"
$Form->validate('input_email', "user@example.com")->email(); 

// Pass

$Form->validate('input_email', "user@example")->email(); 

// Fail

$Form->validate('input_email', "example.com")->email(); 

// Fail
```

```title="Default error message"
Invalid email format
```

```php title="Parameters"
[
	'error' => "Custom error message"
]
```

### Is Filled

Check if `$value` is filled. 

```php
public function isFilled(): bool
{
	...
}
```
The method checks if the value is **empty**, **null** or the **lenght is 0** and if so returns `false`. Spaces does not count as characters. An input containing only spaces is considered empty.

```php title="Example"
$Form->validate('input_field', "Hello World!")->isFilled();

// true

$Form->validate('input_field', " ")->isFilled(); 

// false

$Form->validate('input_field', null)->isFilled(); 

// false
```

This method **does not generate an error message**.

### Length

Check if the length of `$value` is between the given values.

```php
public function length(int $minLength, int $maxLength, array $params = []): object
{
	...
}
```

The `length()` method uses the [`lengthMin()`](#lengthmin) and [`lengthMax()`](#lengthmax) methods for the calculations.

```php title="Example"
$Form->validate("input_field", "Doughnut")->length(5, 10); // length = 8

// Pass
```

You can set the error messages for the min and max values respectively. If you donä, the default values for [`lengthMin()`](#lengthmin) and [`lengthMax()`](#lengthmax) will be used

```php title="Parameters"
[
	'errorMin' => "Custom error message",
	'errorMax' => "Custom error message"
]
```

### Length Max

Check if the length of `$value` is smaller than given value.

```php
public function lengthMax(int $maxLength, array $params = []): object
{
	...
}
```

```php title="Example"
$Form->validate("input_field", "Big Tuna")->lengthMax(10); // length = 8

// Pass
```

```title="Default error message"
Too few characters
```

```php title="Parameters"
[
	'error' => "Custom error message"
]
```

### Length Min

Check if the length of `$value` is greater than given value.

```php
public function lengthMin(int $minLength, array $params = []): object
{
	...
}
```

```php title="Example"
$Form->validate("input_field", "Hi!")->lengthMin(5); // length = 3

// Fail
```

```title="Default error message"
Too few characters
```

```php title="Parameters"
[
	'error' => "Custom error message"
]
```

### Matching

Check if `$value` matches another given value.

```php
public function matching($matchingValue, array $params = []): object
{
	...
}
```

If the values does not match (`$x !== $y`) an error message is created. One example is comparing a password and password confirmation.

```php title="Example"
$password = "pass111";
$password_conf = "word999";

$Form->validate("input_password", $password)->matching($password_conf);

// Fail
```

```title="Default error message"
The values doesn't match
```

```php title="Parameters"
[
	'error' => "Custom error message"
]
```

### Name

Decide if `$value` has name forat.

```php
public function name(array $params = []): object
{
	...
}
```

This method will check if `$value` has the correct length and characters. It can only contain alphabetic unicode characters, spaces and hyphens (-). If it contains any other character or number, the validation will fail.

```php title="Example"
$Form->validate('input_name', "Michael Scott")->name();

// Pass

$Form->validate('input_name', "Mich@e1 $c0tt")->name();

// Fail
```

```title="Default error message"
Invalid name format
```

To limit the length of the name you use the `min` and `max` parameters. You can not use the [`lengthMin()`](#lengthmin), [`lengthMax()`](#lengthmax) or [`length()`](#length) methods with this method.

```php title="Parameters"
[
	'min' => 10, // default: 1
	'max' => 30, // default: 50
	'error' => "Custom error message"
]
```

### Numeric

Check if `$value` ony contains numeric characters (0-9).

```php
public function numeric(array $params = []): object
{
	...
}
```

```php title="Example"
$Form->validate("input_field", 69)->numeric();

// Pass (nice)
```

```title="Default error message"
The field can only contain numeric characters (0-9)
```

```php title="Parameters"
[
	'error' => "Custom error message"
]
```

### Password

Check if `$value` is a valid password.

```php
public function password(array $params = []): object
{
	...
}
```

This method can be modified to fit your password preferences. By default every character is allowed and there is no minimum requirement for the password other than the length.

```php title="Example"
$Form->validate('input_password', "")->password();

// Pass

$Form->validate('input_password', "")->password();

// Fail
```

```title="Default error message"
Invalid password format
```

You can set the length of the password by using the `min` and `max` parameters.

```php title="Parameters"
[
	'min' => 16, // default: 6
	'max' => 128, // default: 50
	'error' => "Custom error message"
]
```

### RegEx

Check if `$value` matched the given regular expression.

```php
public function regex(string $regularExpression, array $params = []): object
{
	...
}
```

```php title="Example"
$Form->validate("input_field", "Hello")->regex('/[^\p{L} -]+/u');

// Pass

$Form->validate("input_field", "Nr#one!")->regex('/[^\p{L} -]+/u');

// Fail
```

```title="Default error message"
The field contains characters that are not allowed
```

```php title="Parameters"
[
	'error' => "Custom error message"
]
```

### Required

Check if `$value` is filled.

```php
public function required(array $params = []): object
{
	...
}
```

The `required()` method uses the [`isFilled()`](#filled-field) method to check if the input has been submitted correctly. If the `$value` is considered empy or null, an error message is generated.

```php title="Example"
$Form->validate('input_field', "Boom, roasted")->required();

// Pass

$Form->validate('input_field', " ")->required();

// Fail
```

```title="Default error message"
The field is required
```

```php title="Parameters"
[
	'error' => "Custom error message"
]
```

### Unique

Check if `$value` is already stored in the database.

```php
public function unique(string $col, string $table, array $params = []): object
{
	...
}
```

Insert the column and table names that should be checked in the database. If `$value` already exists in the given table and column, an error message is generated. 

A common use case is to check if an email address already exists in the database.

```php title="Example"
$Form->validate('input_email', $email)->unique('email_col', 'users_tbl');

// No match in db: Pass
// Match in db: Fail
```

```title="Default error message"
The value already exists
```

The parameter `'ignore'` can be used to ignore a row ID from the search. Example: when a user is updating their profile and their email is already in the db and should not trigger an error message.

```php title="Parameters"
[
	'ignore' => $userId, // default: null
	'error' => "Custom error message"
]
```