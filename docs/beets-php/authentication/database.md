The database comes with four tables that are user with the user authentication and authorization. When making you SQL queries, you can access their names using constands that are defined in [~/config/app.php](../configuration/app.md).

## Tables

The table names can be accessed by constants defined in [~/config/app.php](../configuration/app.md).

```php
define('DB_USER_ACCOUNTS', 'admin__user_accounts');
define('DB_ROLES', 'admin__roles');
define('DB_PERMISSIONS', 'admin__permissions');
define('DB_PERMISSIONS_REL', 'admin__permissions_relations');
```

### admin__user_accounts

This table contains the user accounts and all their info, including encrypted passwords.

### admin__roles

This table contains all of your user roles.

### admin__permissions

This table contains all the different permissions you have in you application.

### amdin__permissions_relations

This table contains the relations betseen the permissions and roles so that the correct permissions are assigned the right roles.	

## Starter-users

There are three users included in the database when you install Beets PHP, one for each user role. The credentials are listed in the table below. You can use these users to setup and test your application.

!!! warning "Make sure to delete these users before publishing your application!"

| Email                 | Password | Role     |
| --------------------- | -------- | -------- |
| sysadmin@sysadmin.com | sysadmin | sysadmin |
| admin@admin.com       | admin    | admin    |
| user@user.com         | user     | user     |