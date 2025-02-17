Beets PHP has a class with methods for form validation. Access them by importing the Form.php class.

## Form.php

```php title="Location"
~/App/Core/Form.php
```

```php title="Namespace"
namespace App\Core;
```

```php title="Import"
use App\Core\Form;
```

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
	// Flash errors and old values to the session
	$Form->flashErrors();

	Session::flashOld([
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
private $field;
private $value;
private $valueLength;

public function validate(string $field, $value): object
{
	...
}
```

- `$field` is used when generating an error message so that you can assign the correct error message to the corresponding form input. 
- `$value` is stored so that the cascading methods like `required()`, `name()` and others can use it to validate it.
- `$valueLength` is the length of `$value` ("hello" = 5).

```html title="Example"
<form>
	<input type="text" name="input_field" value="">
</form>
```
```php title="Example"
use App\Http\Form;

$input = escape($_POST['input_field']); // ""

$Form = new Form();

$Form->validate('input_field', $input)->required();

// Fail (input_filed is required but $input is empty)
```

## Error messages

When a validation fails, an error message is created. The error message is put in an `$errors` array and can be accessed so that the user can get feedback on what data was not submitted correctly.

### Set a message

Use the `error()` method to set an error message and store it in the errors array.

```php
public function error(string $label, $key, $value): object
{
	...
}
```

- `$label` is the identifier of the message, like the name of the form field that was validated.
- `$key` is the name of the validation that was performed.
- `$value` is the error message.

```php title="Example"
$string = "Assistant to the Manager";

if (mb_strlen($string) > 5) {
	$Form->error('input_field', 'titleCheck', "Too long title");
}
```

### Get the messages

Use the `errors()` method to get the whole errors array. If you only want a specific label, you can pass it as an argument.

```php
public function errors($label = null): array
{
	...
}
```

```php title="Example"
$Form->validate('input_field', "")->required();

print_r($Form->errors());

// Output:
[
    ["input_field"] => [
        ["titleCheck"] => "Too long title"
    ]
]

print_r($Form->errors('input_field'));

// Output:
[
    ["titleCheck"] => "Too long title"
]

echo $Form->errors('input_field')['titleCheck']; 

// Output:
Too long title
```

The array can hold many labels ("input_field"), but each label can only hold the latest validadion error. 

Example: if you validate a field that can only be alpha characters and max 10 characters long, but is submitted 20 characters long and containing characters like "&!#", then only the last check will be stored. If that was the length error, the characters error will be shown if the length is shortened.

### Flash errors

The Form class has a method for flashing the error messages to the session, like a shortcut for `Session::flash()`.

```php
public function flashErrors(): void
{
	...
}
```

```php title="Example"
$Form->flashErrors();
```

### Use in view

If the errors method contains errors, you would want to show them in you form. To do this you can use the `error()` and `errorStyle()` functions. You pass the input name as arguments and if the input name is associated with an error, it will be printed where the `error()` function is. The `errorStyle()` function is echoed as a class and will print a bootstrap class styling the form field if there is an error.

```html
<input 
	type="text" 
	class="<?= errorStyle('input_name') ?>" 
	name="input_name"
>
<?= error('input_name') ?>
```

### Persist old data

If the validations fails and the user is redirected back to the form and is presented with the error messages, it would be helpful to see what the faulty input is. 

After we flash the errors with `$Form->flashErrors()` we can flash the input data that we validated to the session. This session data can then be retrieved with the `old()` helper function.

```php title="Example"
$inputEmail = "notanemail";
$Form->validate('input_email', $inputEmail)->email();
$Form->flashErrors();

Session::flashOld([
	'input_email' => $inputEmail,
]);

// In the form view:
<input 
	type="email"
	name="input_email"
	value="<?= old('input_email) ?>"
>

// Output of old('input_email'):
notanemail
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

```php title="Parameters"
[
	'error' => "Only alphabetic characters (a-z, A-Z) allowed" // (string)
]
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

```php title="Parameters"
[
	'error' => "Only alphanumeric characters (a-z, A-Z, 0-9) allowed" // (string)
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

```php title="Parameters"
[
	'error' => "Invalid email format" // (string)
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

You can set parameter `'allowEmpty' => true` if you want the value to be either empty or between an interval. Suitable for fields that are not required but needs limitations if filled.

You can set an error message with the `error` parameter. This message will be used regardless if the value is too small or to large. You can also set the error messages for the min and max values respectively with `errorMin` and `errorMax`. If you don't set any error message, the default values for [`lengthMin()`](#lengthmin) and [`lengthMax()`](#lengthmax) will be used.

```php title="Parameters"
[
	'allowEmpty' => false // (bool)
	'error' => null // (string)
	'errorMin' => null // (string) null = set in lengthMin()
	'errorMax' => null // (string) null = set in lengthMax()
]
```

```php title="Example"
$Form->validate("input_field", "Doughnut")->length(5, 10);

// Pass

$Form->validate("input_field", "")->length(5, 10);

// Fail (the length is 0 but needs to be at least 5)

$Form->validate("input_field", "")->length(5, 10, [
	'allowEmpty' => true
]);

// Pass (the length is allowed to be 0 OR minimum 5)
```

### Length Max

Check if the length of `$value` is smaller than given value.

```php
public function lengthMax(int $maxLength, array $params = []): object
{
	...
}
```

```php title="Parameters"
[
	'error' => "Too few characters" // (string)
]
```

```php title="Example"
$Form->validate("input_field", "Big Tuna")->lengthMax(10); // length = 8

// Pass
```

### Length Min

Check if the length of `$value` is greater than given value.

```php
public function lengthMin(int $minLength, array $params = []): object
{
	...
}
```

```php title="Parameters"
[
	'error' => "Too few characters" // (string)
]
```

```php title="Example"
$Form->validate("input_field", "Hi!")->lengthMin(5); // length = 3

// Fail
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

```php title="Parameters"
[
	'error' => "The values doesn't match" // (string)
]
```

```php title="Example"
$password = "pass111";
$password_conf = "word999";

$Form->validate("input_password", $password)->matching($password_conf);

// Fail
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

To limit the length of the name you use the `min` and `max` parameters. You can not use the [`lengthMin()`](#lengthmin), [`lengthMax()`](#lengthmax) or [`length()`](#length) methods with this method.

If you want the name field to be able to be empty when not `required()`, you must set parameter `'min' => 0` since the default minimum length of a name is 1 character.

```php title="Parameters"
[
	'error' => "Invalid name format" // (string)
	'max' => 64 // (int)
	'min' => 1 // (int)
]
```

```php title="Example"
$Form->validate('input_name', "Michael Scott")->name();

// Pass

$Form->validate('input_name', "Michael Scott")->name(['max' => 5]);

// Fail (name can be max 5 characters)

$Form->validate('input_name', "")->name();

// Fail (name is empty, must be 1-50 characters by default)

$Form->validate('input_name', "")->name(['allowEmpty' => true]);

// Pass (name is allowed to be empty)
```

### Numeric

Check if `$value` ony contains numeric characters (0-9).

```php
public function numeric(array $params = []): object
{
	...
}
```

```php title="Parameters"
[
	'error' => "Only numeric characters (0-9) allowed" // (string)
]
```

```php title="Example"
$Form->validate("input_field", 69)->numeric();

// Pass (nice)
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

You can set the length of the password by using the `min` and `max` parameters or change the the values `PASSWORD_MIN_LENGTH` and `PASSWORD_MAX_LENGTH` in [config.php](./configuration/config.md) to make an app-wide change.

```php title="Parameters"
[
	'error' => "Invalid password format" // (string)
	'max' => 50 // (int)
	'min' => 6 // (int)
]
```

```php title="Example"
$Form->validate('input_password', "password#123!")->password();

// Pass

$Form->validate('input_password', "toby")->password();

// Fail (password must be min 6 characters bu default)
```

### RegEx

Check if `$value` matched the given regular expression.

```php
public function regex(string $regularExpression, array $params = []): object
{
	...
}
```

```php title="Parameters"
[
	'error' => "The field contains characters that are not allowed" // (string)
]
```

```php title="Example"
$Form->validate("input_field", "Hello")->regex('/[^\p{L} -]+/u');

// Pass

$Form->validate("input_field", "Nr#one!")->regex('/[^\p{L} -]+/u');

// Fail (# and ! are not allowed)
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

```php title="Parameters"
[
	'error' => "The field is required" // (string)
]
```

```php title="Example"
$Form->validate('input_field', "Boom, roasted")->required();

// Pass

$Form->validate('input_field', " ")->required();

// Fail
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

The parameter `'ignore'` can be used to ignore a row ID from the search. Example: when a user is updating their profile and their email is already in the db and should not trigger an error message.

```php title="Parameters"
[
	'error' => "The value already exists" // (string)
	'ignore' => null, // (int) Row ID
]
```

```php title="Example"
$Form->validate('input_email', $email)->unique('email_col', 'users_tbl');

// Pass if no match in db
// Fail if match in db
```