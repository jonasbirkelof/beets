# dotenv.php

This file is loaded using the Composer autoloader in `/src/composer.json` and enables the use of the env library throughout the application. The file is located inside the `/src/config/` folder.

Read more about how the required variables work on [vlucas's GitHub](https://github.com/vlucas/phpdotenv).

```php title="/src/config/dotenv.php"
use Dotenv\Dotenv;

// Set the location of .env (shold be app root)
$dotenv_location = __DIR__ . '../../';

$dotenv = Dotenv::createImmutable($dotenv_location);
$dotenv->load();

// Keys that must exist in .env and also can not be empty
$dotenv->required([
	'DB_CONNECTION',
	'DB_HOST',
	'DB_PORT',
	'DB_DATABASE',
	'DB_USERNAME',
])->notEmpty();

// Keys that must exist in .env but can be empty
$dotenv->required([
	'APP_ENV',
	'APP_DEBUG',
	'DB_PASSWORD',
]);
```