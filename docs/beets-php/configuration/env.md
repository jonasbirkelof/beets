# .env

This file contains the environmental variables that are used throughout the application. The library used for the env handlig is [vlucas PHP dotenv](https://github.com/vlucas/phpdotenv). The file is located at the project root and is loaded via the [`~/config/dotenv.php`](./dotenv.md) file.

Beets PHP comes with an example file named `.env.example` that you can modify to your needs.

You can get more detailed descriptions and more functionality by visiting [vlucas's GitHub](https://github.com/vlucas/phpdotenv).

```php title="~/.env.example"
APP_NAME="My App"
APP_DESCRIPTION="This is my app"
APP_ID=1
APP_ENV=local
APP_DEBUG=true
APP_URL=http://myapp.local
APP_COPYRIGHT="The Owner"

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=dbname
DB_CHARSET=utf8mb4
DB_USERNAME=username
DB_PASSWORD=password
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

The `DB_*` variables are used within the [`Database.php`](../classes/Databse.md) class that handles your database requests so changing these variable names can break things.

Every `DB_*` variable except `DB_CHARSET` is required to exist in the file as configured in the [`~/config/dotenv.php`](./dotenv.md) file.

## Usage

You can access your env variables uwing the `$_ENV` or `$_SERVER` super globals:

```php
$appName = $_ENV['APP_NAME'];
$appName = $_SERVER['APP_NAME'];
```