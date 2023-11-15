# app.php

This file is for storing app-wide constants and variables that doesn't fit in the [`.env`](./env.md) file. The file is located inside the `~/config/` folder.

```php title="~/config/app.php"
define('APP_ROOT', $_SERVER['DOCUMENT_ROOT'] . '/../');
define('APP_URL', $_ENV['APP_URL']);
define('APP_ID', $_ENV['APP_ID']);

define('PUBLIC_ROOT', $_SERVER['DOCUMENT_ROOT'] . '/');
```