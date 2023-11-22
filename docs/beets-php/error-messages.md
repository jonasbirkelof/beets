The Error class handles incomming error messages and stores them in an array.

## Error.php

```php title="Location"
~/app/core/Error.php
```

```php title="Namespace"
namespace App\Core;
```

```php title="Import"
use App\Core\Error;
```

## Store a message

Use the `set()` method to store an error message in the errors array.

```php
public static function set(string $label, $key, $value): object
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
	Error::set('error', 'titleCheck', 'Too long title');
}
```

## Get the messages

Use the `get()` method to get the whole errors array. If you only want a specific label, you can pass it as an argument.

```php title="Example"
// Set the error message
Error::set('error', 'titleCheck', 'Too long title');

print_r(Error::get());

// Output:
[
    ["error"] => [
        ["titleCheck"] => "Too long title"
    ]
]

print_r(Error::get('error'));

// Output:
[
    ["titleCheck"] => "Too long title"
]

echo $Form->errors('error')['titleCheck']; 

// Output:
Too long title
```