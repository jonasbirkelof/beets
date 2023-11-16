If you want to add our own custom functions yo can use the file `functions.php` that is located in the `~/app/helpers/` folder. We separate the core app functions and the custom user functions for safety reasons.

This file is automatically included via the autoloader.

## Examples

Below are some examples of functions that you might want to put here.

### Access array in data.php

If you have added an array with static data that should be used across the app, you might want to build a function to easily access it:

```php title="~/config/data.php"
$lookup = [
	0 => 'zero',
	1 => 'one',
	2 => 'two'
];
```

```php title="~/app/helpers/functions.php"
function lookup($input = null)
{
	require APP_ROOT . '/config/data.php';

    return $lookupTable[$input] ?: "No input submitted";
}
```

```php
echo lookup(1); // one
echo lookup(); // No input submitted
```

### Convert checkbox state

```php title="~/app/helpers/functions.php"
function checkboxState($input)
{
	return $input === 1 ? "checked" : null;
}
```

```php
$state = checboxState(1); // Value 1 can come from a db

echo "<input type=\"checkbox\" $state>"; // <input type="checkbox checked">
```