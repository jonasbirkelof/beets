The profile page is the page that the logged in user can change its information. Any user can access this page regardless of user role. The default layout is the same as the edit user page but missing is the ability to change user role and to deactivate the account. 

This is important because you have the possibility to limit what the logged in users can do. If you don't want them to be able to update the email address, you can disable this for the profile page by changing the input field to a read-only field and removing the column from the SQL query in the model.

The Profile model and ProfileController helps you with you actions for this page.

## ProfileController

```php title="Location"
~/App/Http/Controllers/ProfileController.php
```

```php title="Namespace"
namespace App\Http\Controllers;
```

```php title="Import"
use App\Http\Controllers\ProfileController;
```

The ProfileController is small and contains only of the `update()` method that updates the profile, and `edit()` method that shows the profile page.

## Profile model

```php title="Location"
~/App/Models/Profile.php
```

```php title="Namespace"
namespace App\Models;
```

```php title="Import"
use App\Models\Profile;
```

### User data

To get the user data when showing the profile page, we can use the `find()` or `findOrFail()` methods. None of these methods take an argument or a user ID since they should only return values for the logged in user. They both use `User::id()` toghether with `User::find()` and `User::findOrFail()` to get the user data. You can read about how these methods work on the [User methods page](./user-methods.md#get-any-users-data).

```php title="ProfileController.php" hl_lines="3"
public static function edit()
{
	$user = Profile::findOrFail();

	return App::view('profile.php', ['user' => $user]);
}
```

```php title="Profile.php"
public static function findOrFail()
{
	return User::findOrFail(User::id());
}
```

When a user is updating the account information, the `Profile::update()` method is run. The method validates the input fields, executes the SQL query and updates the user session with the new information.

```php title="ProfileController.php"
if (! Profile::update($_POST)) {
	Redirect::to("/profile")->with('feedback', 'profile_update_failed');
}
```

### Profile image

The user can change the profile image and the ProfileController uses the User model to do this. By using `User::updateImage()` and `User::deleteImage()` we get the same functionality as an admin.

```php title="ProfileController.php"
if (! User::updateImage(User::id())) {
	Redirect::to("/profile")->with('feedback', 'image_update_failed');
}

if (! User::deleteImage(User::id())) {
	Redirect::to("/profile")->with('feedback', 'image_delete_failed');
}
```

### Password

When the users change their password we use the same method that the "forgot password?" and admin pages uses to get consistance and the same level of security.

```php title="ProfileController.php"
if (! Password::update(
	$_POST['password'], 
	$_POST['password_repeat'], 
	User::id()
)) {
	Redirect::to("/profile")->with('feedback', 'password_update_failed');
}
```