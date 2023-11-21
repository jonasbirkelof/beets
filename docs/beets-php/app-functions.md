The app functions are functions that comes natively with Beets PHP. The file is located in the `~/app/core/` folder and is loaded automatically via the autoloader.

The `/includes` folder is used for lager functions or functions that belong together but does not need to be in a class.

```php title="~/app/core/functions.php"
// Die-and-dump function
require_once 'includes/dd.php';

// Authentication functions
require_once 'includes/auth.php';

// Core functions

function escape($input)
{
	$string = htmlentities($input);
	$string = htmlspecialchars($input);
	$string = strip_tags($input);
	
	return $string; 
}

function view($pattern, $attributes = [])
{
	return \App\Core\App::view($pattern, $attributes);
}

function old($key, $default = '')
{
	return \App\Core\Session::get('old')[$key] ?? $default;
}
```