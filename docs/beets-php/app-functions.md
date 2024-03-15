The app functions are functions that comes natively with Beets PHP. The file is located in the `~/app/core/` folder and is loaded automatically via the autoloader.

The `/includes` folder is used for lager functions or functions that belong together but does not need to be in a class.

```php title="~/app/core/functions.php"
// Die-and-dump function
require_once 'includes/dd.php';

// Authentication functions
require_once 'includes/auth.php';

// Core functions

/**
 * Escape HTML and special characters from a string.
 */
function escape($input)
{
	...
}

/**
 * Return a view and pass attributes that will be accessible in the view as an array.
 */
function view($pattern, $attributes = [])
{
	...
}

/**
 * Execute the Feedback::run() method.
 */
function feedback()
{
	...
}

/**
 * Return errors stored in the session and print them using Bootstrap.
 */
function error($field)
{
    ...
}

/**
 * Style an input with Bootstrap if there are errors.
 */
function errorStyle($field)
{
	...
}

/**
 * Return the "old" form value for an input from the session.
 */
function old($key, $default = '')
{
	...
}

/**
 * Print a hidden input field and set the name and value of it.
 */
function hidden(string $name, string $value)
{
	...
}

/**
 * Print a hidden "_method" field to set a custom HTTP request method.
 */
function method(string $requestMethod)
{
	...
}

/**
 * Print a hidden CSRF field to a form.
 */
function csrf()
{
	...
}

/**
 * Return a file from the path to the folder "~/storage"
 */
function storage(string $filename)
{
	...
}

/**
 * Return a file from the path to the folder "~/public/assets/images"
 */
function image(string $filename)
{
	...
}

/**
 * Return a file from the path to the folder "~/public/partials
 */
function partial(string $filename, string $extension = DEFAULT_FILE_EXTENSION)
{
	...
}

/**
 * Check the app environment return as bool or string.
 */
function env(string $environment)
{
	...
}

/**
 * Translate APP_DEBUG into bool.
 */
function debug()
{
	...
}
```