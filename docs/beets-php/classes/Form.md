# Form.php

This class contains methods for form validation and handling error messages.

## Location

```php
~/app/http/Form.php
```

## Namespace

```php
namespace App\Http;
```

## Import the class

```php
use App\Http\Form; // Import the Form class
```

## Dependencies

```php
use App\Core\Database as DB; // Import the Database class
use App\Core\Session; // Import the Session class
```

## Properties

```php
protected $errors = [];
private $field;
private $value;

// Default error messages
private const FAULTY_VALUE = "Incorrect value";
private const EMAIL = "Incorrectly formatted e-mail";
private const MATCHING = "The values doesn't match";
private const NAME = "Incorrectly formatted name";
private const PASSWORD = "Incorrectly formatted password";
private const REQUIRED = "The field is required";
private const UNIQUE = "The value already exists";
```

## Methods

### errors()

```php
public function errors(): array
{
	return $this->errors;
}
```

### error()

```php
public function error($check, $field, $message): void
{
	$this->errors[$field] = [$check => $message];
}
```

### validate()

```php
public function validate($field, $value): object
{
	$this->field = $field;
	$this->value = $value;
	return $this;
}
```

### isFilled()

```php
public function isFilled(): bool
{
	$isFilled = true;

	if ($this->value == "") $isFilled = false;
	if ($this->value == null) $isFilled = false;
	if (strlen(trim($this->value)) == 0) $isFilled = false;

	return $isFilled;
}
```

### required()

```php
public function required($errorMessage = self::REQUIRED): object
{
	if (! $this->isFilled()) {
		$this->error('required', $this->field, $errorMessage);
	}

	return $this;
}
```

### name()

```php
public function name($minLength = 1, $maxLength = 50, $errorMessage = self::NAME): object
{
	if (
		mb_strlen($this->value) < $minLength
		|| mb_strlen($this->value) > $maxLength
		|| preg_match('/[^\p{L} -]+/u', $this->value) === 1
	) {
		$this->error('name', $this->field, $errorMessage);
	}

	return $this;
}
```

### email()

```php
public function email($errorMessage = self::EMAIL): object
{
	$valueLength = mb_strlen($this->value);

	if ($valueLength > 0) {
		$validatedEmail = filter_var($this->value, FILTER_VALIDATE_EMAIL) ? true : false;

		if (! $validatedEmail) {
			$this->error('email', $this->field, $errorMessage);
		}
	}

	return $this;
}
```

### unique()

```php
public function unique($col, $table, $ignoreId = null, $errorMessage = self::UNIQUE): object
{
	$valueLength = mb_strlen($this->value);

	if (! $this->isFilled()) {
		$errorMessage = self::FAULTY_VALUE;
	}

	if ($valueLength > 0) {
		if ($ignoreId) {
			$sql = "SELECT $col FROM $table WHERE $col = ? AND id <> ?";
			$result = DB::query($sql, [$this->value, $ignoreId])->fetchAll();
		} else {
			$sql = "SELECT $col FROM $table WHERE $col = ?";
			$result = DB::query($sql, [$this->value])->fetchAll();
		}

		if (count($result) > 0) {
			$this->error('unique', $this->field, $errorMessage);
		}
	}		

	return $this;
}
```

### password()

```php
public function password($minLength = 6, $maxLength = 50, $errorMessage = self::PASSWORD): object
{
	$errorMessage = "Lösenordets längd måste vara mellan $minLength och $maxLength tecken";

	if (mb_strlen($this->value) < $minLength || mb_strlen($this->value) > $maxLength) {
		$this->error('password', $this->field, $errorMessage);
	}

	return $this;
}
```

### matching()

```php
public function matching($matchingValue, $errorMessage = self::MATCHING): object
{
	if ($this->value !== $matchingValue) {
		$this->error('matching', $this->field, $errorMessage);
	}

	return $this;
}
```