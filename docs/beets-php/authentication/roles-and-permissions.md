By giving the users different roles you can limit what they can do in your app. The roles have different permissions assigned to them to make it easy for you to fine tune the limitations. 

The [starter-users](./database.md#starter-users) have the roles user, admin and sysadmin assigned to them so that you can test your permissions and roles. These are the default permissions for each role, but feel free to thange them as you need!

| Permisson       |              syadmin              |               admin               |               user                |
| --------------- |:---------------------------------:|:---------------------------------:|:---------------------------------:|
| view_users_list | <i class="fa-solid fa-check"></i> | <i class="fa-solid fa-check"></i> | <i class="fa-solid fa-check"></i> |
| view_user       | <i class="fa-solid fa-check"></i> | <i class="fa-solid fa-check"></i> | <i class="fa-solid fa-check"></i> |
| add_user        | <i class="fa-solid fa-check"></i> | <i class="fa-solid fa-check"></i> |                                   |
| edit_user       | <i class="fa-solid fa-check"></i> | <i class="fa-solid fa-check"></i> |                                   |
| delete_user     | <i class="fa-solid fa-check"></i> |                                   |                                   |

The relation between the roles and permissions are set in the table `admin__permissions_relations` in the database.

## Helper functions

You can limit actions by user roles or permissions with these helper functions.

```php
if (permission('add_user')) {
	echo "<button>Add User</button>";
}
```

All users that has the permission "add_user" assigned to their role will see the button. This is effective if you need multiple roles to access an action. You can also build a UI to change what permissions belongs to which roles for  increased flexibility. This even opens up for you to create custom roles on the go by making an interface for setting up this function.

The `permissions()` function checks if the permission (add_user in this case) exists in the array of permissions for the logged in user (`Auth::permissions()`).

```php
if (role('admin')) {
	echo "<button>Add User</button>";
}
```

This way you can hide the button for all users that are not admins. You can also use any of these functions instead of `role()` to shorten the code and make it more readable. All these functions uses the `Auth::hasRole()` method to check if the logged in user is assigned the given role.

- `#!php user()`
- `#!php admin()`
- `#!php sysadmin()`

```php title="PHP Example"
if (permission('add_user')) { echo "Add user"; }

if (sysadmin()) { echo "Delete user"; }
if (sysadmin() || admin()) { echo "Edit user"; }
```

```html title="HTML Example"
<?php if (permission('add_user)) : ?>
<button>Add User</button>
<?php endif; ?>

<?php if (sysadmin()) : ?>
<button>Delete User</button>
<?php endif; ?>

<?php if (sysadmin() || admin()) : ?>
<button>Edit User</button>
<?php endif; ?>
```

## Permission methods

```php title="Location"
/src/App/Models/Authenticate/Permission.php
```

```php title="Namespace"
namespace App\Models\Authenticate;
```

```php title="Import"
use App\Models\Authenticate\Permission;
```

The Permission model proveides you with a couple of methods for retrieving a user permissions.

You can get an array of multiple permissions or a smaller array with only one permission depending on your needs.

You can get a specific permission data with the `find()` method and the permission ID. This method will return an array of data.

```php
$permission = Permission::find(123);
```

You can use the `findByRole()` method, that takes a role ID, if you want to get all permissions with all data that belongs to a given role. Since the role ID is set on the user, it's easy to get the role ID with `User::role()` and pass it in the method to get all permissions that are given to a user. 

This method is used to get the permissions data when logging in so if you want the permissions for the logged in user, just use `User::permissions()` for better performance.


 return a 404-error page if the user ID does not match any user in the database. The `find()` method is used to check for the user in the database.

```php
$permission = User::findByRole(456);
```

To get an array of multiple permissions with their data, you can use the `get()` method. 

```php
$permissions = Permission::get();
```

This method can recieve two arguments to help you filter and sort your results; `orderBy` and `where`. These arguments are inserted as strings at the end of the query. Below are som examples of how to use them. This is also described on the [Database workflow page](../database.md#database-methods).

```php
$permissions = Permissions::get(
	['where' => ['name = add_user']]
);

$permissions = Permissions::get(
	['orderBy' = ['name ASC']]
);
```

## Role methods

```php title="Location"
/src/App/Models/Authenticate/Role.php
```

```php title="Namespace"
namespace App\Models\Authenticate;
```

```php title="Import"
use App\Models\Authenticate\Role;
```

The Role model proveides you with a couple of methods for retrieving role data.

You can get an array of multiple roles or a smaller array with only one role depending on your needs.

You can get a specific role data with the `find()` method and the role ID. This method will return an array of data.

```php
$role = Role::find(123);
```

To get an array of multiple roles with their data, you can use the `get()` method. 

```php
$roles = Role::get();
```

This method can recieve two arguments to help you filter and sort your results; `orderBy` and `where`. These arguments are inserted as strings at the end of the query. Below are som examples of how to use them. This is also described on the [Database workflow page](../database.md#database-methods).

```php
$roles = Role::get(
	['where' => ['name = admin']]
);

$roles = Role::get(
	['orderBy' = ['name ASC']]
);
```