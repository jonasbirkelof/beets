# Cheat sheet

Quick access to some useful tools without extended explanation.

## Database

### Select

Multiple posts:

```php
use App\Core\Database as DB;

$sql = "SELECT * FROM table";
$result = DB::query($sql)->fetchAll();
```

Single post:

```php
$sql = "SELECT * FROM table WHERE id = :id";
$result = DB::query($sql, ['id' => $id])->fetch();
```

Filter (MVC example):

```php title="UserController.php"
use App\Models\User;

$users = User::get(
	'where' => ['status = 1'],
	'orderBy' => ['first_name ASC'],
);

return App::view('/users/index.php', [
	'users' => $users
]);
```

```php title="User.php"
$orderClause = DB::orderBy(! empty($args['orderBy']) ? $args['orderBy'] : []);
$whereClause = DB::where(! empty($args['where']) ? $args['where'] : []);

$sql = "SELECT first_name, last_name FROM users $whereClause $orderClause";
$result = DB::query($sql)->fetchAll();

return $result ?: [];
```

```php title="index.php"
print_r($users);
```

### Insert into

```php
$sql = "INSERT INTO table (col_1, col_2) VALUES (:val_1, :val_2)";
$result = DB::query($sql, ['val_1' => 'foo', 'val_2' => 'bar']);
```

### Update

```php
$sql = "UPDATE table SET col_1 = :val_1, col_2 = :val_2 WHERE id = :id";
$result = DB::query($sql, ['val_1' => 'foo', 'val_2' => 'bar', 'id' => 123]);
```

### Delete

```php
$sql = "DELETE FROM table WHERE id = :id";
$result = DB::query($sql, ['id' => 123]);
```

## Forms

### CSRF

Create and validate token (in controller):

```php
use App\Core\CSRF;

// Create new token before form load
CSRF::newToken();

// Validate after form submit [bool]
CSRF::validate()
```

Create hidden form field (in view):

```html
<?= csrf() ?>
```

### Validation

Validate a form input (in model):

```php
use App\Http\Form;

$Form = new Form();

// Validate an input
$Form->validate('input_name', $_POST['input_name'])->required();

// Check for errors/validation fails
if (! $Form->errors()) {
	// No errors, save to db
	...
}

// Flash errors to session
$Form->flashErrors();

// Flash old data to session
$Form->flashOld([
	'input_name' => $_POST['input_name']
]);
```

Echo the errors and style the input (in view) if there are any:

```html
<input 
	type="text" 
	class="<?= errorStyle('input_name') ?>" 
	name="input_name"
>
<?= error('input_name') ?>
```

Validation methods and their parameters:

```php
alpha(['error'])
alphaNumeric(['error'])
email(['error'])
isFilled()
length(['allowEmpty', 'error', 'errorMin', 'errorMax'])
lengthMax(['error'])
lengthMin(['error'])
matching(['error'])
name(['error', 'max', 'min'])
numeric(['error'])
password(['error', 'max', 'min'])
regex(['error'])
required(['error'])
unique(['error', 'ignore'])
```

## Feedback

```php
use App\Http\Feedback;

// Set feedback name
Feedback::set('save_success');

// Create the component
Feedback::create('save_success')->toast([
    'text' =>, 'The post was saved',
    'style' =>, 'success',
    'icon' =>, 'fa-solid fa-check-circle'
]);

// Run the feedback message
Feedback::run();
```

Feedback types and their properties:

```php
alert(['text', 'style', 'icon'])
toast(['text', 'style', 'icon', 'offset-y'])
```

Echo out the feedback component (in view) if there is one:

```html
<?= feedback() ?>
```

## Redirecting

```php
use App\Core\Redirect;

Redirect::to('/page');
```

### With flash message

```php
Redirect::to('/page')->with('message', 'This is a message');

Session::get('message'); // This is a message
```

### Request URI

Use the request URI that is in session

```php
Redirect::to('/page')->preferRequestUri();
```

## Sessions

Flash messages (`flash()`) are deleted after every page reload. Regular session values (`put()`) are deleted at logout.

```php
use App\Core\Session;

Session::flash('message', 'Hello World'); 

// $_SESSION['_flash']['message'] = "Hello World"

Session::put('name', 'Dwight');

// $_SESSION['name'] = "Dwight"
```

Get the values:

```php
echo Session::get('message'); // Hello World
echo Session::get('name'); // Dwight
```

Check if key exists:

```php
Session::has('message'); // true
Session::has('name'); // true
```

Empty the session:

```php
Session::flush(); // Empty the whole session
Session::flush('name') // Empty only the key "name"

Session::destroy(); // Destroy the session
```