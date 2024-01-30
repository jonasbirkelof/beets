# About

Beets PHP comes with a fully functional user authentication and authorization system with permissions, user roles, password reset and profile page with the ability to upload a profile picture.

## Database

The database comes with four tables that are user with the user authentication and authorization. When making you SQL queries, you can access their names using constands that are defined in [~/config/app.php](../configuration/app.md).

### admin__user_accounts

This table contains the user accounts and all their info, including encrypted passwords.

PHP constant: `DB_USER_ACCOUNTS`.

### admin__roles

This table contains all of your user roles.

PHP constant: `DB_ROLES`.

### admin__permissions

This table contains all the different permissions you have in you application.

Constant: `DB_PERMISSIONS`.

### admin__permissions_relations

This table contains the relations betseen the permissions and roles so that the correct permissions are assigned the right roles.	

Constant: `DB_PERMISSIONS_REL`.

## Routes and assets

There are a couple of routes registered in `~/routes/web.php` that are used for administate and access your user accounts. There are also some controllers, models and views to help you get started.

### Views

All views ar located in `~/public/views/authenticate` and `~/public/views/users`, except for profile that is located in `~/public/views`

URLs:

* Login form: `/login` or `/` when not logged in.
* Reset password form: `/reset-password`
* Select new password form: `/reset-password/new?token=` (only accesseble when a password reset token is genereated).
* List all users: `/users`
* Create user: `/users/create`
* Show user: `/users/{userId}`
* Edit and delete user: `/users/{userId}/edit`
* Profile page: `/profile`

## Starter-users

There are three users included in the database when you install Beets PHP, one for each user role. Credentials are in the table below. You can use these users to setup and test your application.

!!! warning "Make sure to delete these users before publishing your application!"

| Email                 | Password | Role     |
| --------------------- | -------- | -------- |
| sysadmin@sysadmin.com | sysadmin | sysadmin |
| admin@admin.com       | admin    | admin    |
| user@user.com         | user     | user     |



## List users

This view is created for you to get a quick overview of all your user accounts.