## Get user data

You can get a user's data by multiple ways depending on what you need.

### Logged in user

If you want the data of the logged in user, there are two ways. At the core, the Authenticate class methods will return any data in the user session.

```php title="Example"
use App\Core\Authenticate as Auth;

$user = Auth::user(); // Array
```

```php
Auth::id();
Auth::firstName();
Auth::lastName();
Auth::fullName();
Auth::initials();
Auth::email();
Auth::image();
Auth::role(); // Array
Auth::permissions(); // Array
```

### Update logged in user data

The user session is created and populated when logging in. If you need to update the data during the session, you can do so with the `set()` method:

```php
use App\Core\Authenticate as Auth;

Auth::set('last_name', 'McNewname');
```

## Logged in or specific user

You can also use the User class to get the same data as the Authenticate class. 

```php title="Example"
use App\Models\User;

$user = User::session(); // Array
```

The thing that differs is that you pass a user ID in the methods (not the `session()` method though). Then you will instead get the data of that perticular user. 

```php
User::firstName(); // For logged in user
User::firstName(123); // For user with ID 123
```

These methods are available with the User class and all can pass a user ID:

```php
User::id();
User::firstName();
User::lastName();
User::fullName();
User::initials();
User::image();
User::role(); // Array
User::permissions(); // Array
```

Of course these methods are only meant for retrieving the ocational small data. If you want to get all of the users data, thera are [other ways](#get-any-users-data) that don't require so many querys to be executed.

## Get any users data

You can get an array consisting of multiple users or a smaller array with only one user depending on your needs.

You can get a specific user's data with the `find()` method and the user ID. This method will return an array of data.

```php
$user = User::find(123);
```

You can use the `findOrFail()` method if you want to return a 404-error page if the user ID does not match any user in the database. The `find()` method is used to check for the user in the database.

```php
$user = User::findOrFail(123);
```

To get an array of multiple users, you can use the `get()` method. 

```php
$users = User::get();
```

This method can recieve two arguments to help you filter and sort your results; `orderBy` and `where`. These arguments are inserted as strings at the end of the query. Below are som examples of how to use them. This is also described on the [Database workflow page](../database.md#database-methods).

```php
$users = User::get(
	['where' => ['status = 1']]
);

$users = User::get(
	['orderBy' = ['first_name ASC']]
);

$users = User::get(
	['where' => ['status = 1', 'role = 2']],
	['orderBy' => ['first_name ASC', 'last_name DESC']]
);
```

## Helpers

The class also contains a couple of helper methods that are used with the other methods or the `UserController`.

### Store, update and delete

The `store()` and `update()` methods takes the form POST data and returns `true` if successful and `false` if not. `destroy()` takes the user ID that should be deleted and return `true` if successful.

```php
User::store($_POST);
User::update($_POST);
User::destroy(123);
```

### Profile images

To update and delete the user profile image, we can use the `imageDelete()` and `imageUpdate()` methods that takes the user ID that owns the image. These are used in multiple places as well as the `ProfileController`.

Deleting the image removes it from the storage and from the user account column in the database. The session data will be udated when this is done so the user does not have to log out and in to see the changes.

When updating, the method can grab the file data directly from the `$_FILE` superglobal. It creates a new filename using the user ID and a timestamp using the `profileImageName()` helper function.

```php
User::deleteImage(123);
User::updateImage(123);
```

### Initials

The `makeInitials()` method thakes a first name and a last name and returns the initials.

```php
echo User::makeInitials('Dwight', 'Schrute'); // DS
```

