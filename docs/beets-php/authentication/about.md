Beets PHP comes with a fully functional user authentication and authorization system with permissions, user roles, password reset and profile page with the ability to upload a profile picture.

By working with the permissions and roles functions like `admin()` and `permission('edit_user')` you can easily control what parts of the UI and backend are accesseble to the users.

When a user has forgotten its password, an email is sent to the registered user account email with instructions on how to set a new password. The link is only active for a limited time and can only be used once.