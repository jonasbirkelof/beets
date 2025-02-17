# .env

This file contains the environmental variables that are used throughout the application. The library used for the env handlig is [vlucas PHP dotenv](https://github.com/vlucas/phpdotenv). The file is located at the project root and is loaded via the [`/src/config/dotenv.php`](./dotenv.md) file.

Beets PHP comes with a pre-configured `.env` file that you can modify to your needs.

You can get more detailed descriptions and more functionality by visiting [vlucas's GitHub](https://github.com/vlucas/phpdotenv).

```php title="/src/.env"
APP_NAME="Beets PHP"
APP_DESCRIPTION="A simple Beets PHP application"
APP_ID=1
APP_ENV=development
APP_DEBUG=true
APP_URL=http://localhost:9000
APP_COPYRIGHT="Copyright Owner"

DB_CONNECTION=mysql
DB_HOST=mysql_db
DB_PORT=3306
DB_DATABASE=beetsphp
DB_CHARSET=utf8mb4
DB_USERNAME=root
DB_PASSWORD=root

# DB2_CONNECTION=
# DB2_HOST=
# DB2_PORT=
# DB2_DATABASE=
# DB2_CHARSET=
# DB2_USERNAME=
# DB2_PASSWORD=

DEFAULT_DB=DB

CSRF_SECRET_KEY=secretkey

EMAIL_SENDER=noreply@example.com
```

## .gitignore

Since this file can contain secrets as API credentials, password, internal paths, etc it should never be part of version control. Add the file to your `.gitignore` file.

## Variables

### Nesting

You can nest your env variables to reduce repeated values, like so:

```php
APP_SLUG="myapp"
APP_URL=http://${APP_SLUG}.com
```

Read more on [vlucas's GitHub](https://github.com/vlucas/phpdotenv#nesting-variables).

### APP_

The `APP_*` variables can be used to store values that are app specific and not related to data or content. For instance the applications name, url or author.

### DB_

The `DB_*` variables are used within the Database.php class that handles your database requests so changing these variable names can break things.

Every `DB_*` variable except `DB_CHARSET` is required to exist in the file as configured in the [`/src/config/dotenv.php`](./dotenv.md) file. The additional database variables that are commented out by default optional.

## Usage

You can access your env variables uwing the `$_ENV` or `$_SERVER` super globals:

```php
$appName = $_ENV['APP_NAME'];
$appName = $_SERVER['APP_NAME'];

echo $appName; // My App
```

Using them publicly can ba a safety issue though, so every env variable is defined as a constant in the `/src/config/config.php` file. If you add custom env variables, make sure to add them to the config file.

```php
echo APP_NAME; // My App
```

## Environment

You can set the appllication environment with the variable `APP_ENV`. By default it is set to `development` but when you deploy you application to production, make sure to set it to `production`. These variables can be used to change the application behaviour, style or functionality depending on its location. There are also a `env()` helper function that you can use to get the set environment.

You can read mor on how to use the environment variable on the [Deploy page](../deploy.md).

```php title="/src/.env"
APP_ENV=development
```

```php
if (env('development')) {
	// Development functionality
}

if (env('production')) {
	// Production functionality
}

echo env(); // developemnt
```

Along the `env()` function is the `debug()` function that retrieves the `APP_DEBUG` env variable and translates it into a boolean. Use this to toggle functionality that should only be used for debugging.

```php title="/src/.env"
APP_DEBUG=true
```

```php
if (debug()) {
	// Debug funtionality
}
```