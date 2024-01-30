# app.php

This file is for storing app-wide constants and variables that doesn't fit in the [`.env`](./env.md) file. The file is located inside the `~/config/` folder.

```php title="~/config/app.php"
// App
define('APP_ROOT', $_SERVER['DOCUMENT_ROOT'] . '/..');
define('APP_URL', $_ENV['APP_URL']);
define('APP_ID', $_ENV['APP_ID']);

// Paths
define('PUBLIC_ROOT', $_SERVER['DOCUMENT_ROOT'] . '/');
define('PUBLIC_URL', APP_URL . '/public/');
define('STORAGE', PUBLIC_ROOT . 'storage/');

// Password
define('PASSWORD_MIN_LENGTH', 4);
define('PASSWORD_MAX_LENGTH', 50);
define('PASSWORD_RESET_TOKEN_EXPIRATION', 3600); // seconds

// Database 
define('DB_USER_ACCOUNTS', 'admin__user_accounts');
define('DB_ROLES', 'admin__roles');
define('DB_PERMISSIONS', 'admin__permissions');
define('DB_PERMISSIONS_REL', 'admin__permissions_relations');

// Misc
define('PROFILE_IMAGE_MAX_SIZE', 10000000); // 10000000 = 10 MB
```