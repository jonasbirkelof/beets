A view is a page that is viewed by the visitor. The views should contain as little programming logic as possible. That responsibility lays with the controller that is loading the view by instructions from the router.

The views are located in the `~/public/views` folder. This folder contains the pages that your application has, including the error pages (404-pages and such) and [authentication pages](./authentication/routes-and-views.md).

## Loading a view

When a request to load a page, for instance `/about`, is made, the router decides how to handle that request. The about page could have a controller that would collect information from a database and then use the `App::view()` method to load the page and pass the database data. It is however not necessary to have a controller if the controller's only task is to load a view. See the examples below.

```php title="Example with a controller"
// In the router:
$Router->get('/about', [AboutController::class, 'index']);

// In the AboutController:
public static function index() {
	return App::view('about.php');
}
```

```php title="Example with only the router"
// In the router:
$Router->get('/about', function() {
	return App::view('about.php');
});
```

We can however also pass attributes with the `view()` method. By processing data in the controller or model and then pass it to the view, the view is not cluttered with a lot of PHP code. The second argument of the `view()` method is an array:

```php title="UsersController.php"
public static function index() {
	// Get all users as an array
	$usersList = User::get();

	return App::view('users.php', [
		'users' => $usersList
	]);
}
```

The key 'users' is extracted and made into a variable `$users` that the view can use.

```html title="index.php"
<ul>
	<?php foreach ($users as $user) : ?>
		<li>User name: <?= $user['name'] ?></li>
	<?php endforeach; ?>
</ul>
```

We can also use the `view()` helper function in the same manner should it be needed as it uses the `App::view()` method.

If we need to access the views folder within our code, we can use the `VIEWS` constant:

```php
echo VIEWS; // $_SERVER['DOCUMENT_ROOT'] . APP_FOLDER . '/public/views'
```

## Partials

To load recurring elements in a view, such as the HTML head, a sidebar or a navigation we use what is called "partials". A partial is a snippet of code that can be included in a view. The partials files are located in the `~/public/partials` folder.

To load a partial you pass the partial name (without file extension) in the `parial()` helper function. This code will load the file ´page-head.php´ from the `~/public/partials` folder:

```html
<?php partial('page-head') ?>
```

If you happen to use a file extension that is not ".php", you can change this with the constant `DEFAULT_FILE_EXTENSION` in the `~/config/config.php` file.

Other partials that are included with Beets PHP by default are:

- copyright
- footer
- navigation
- page-foot
- page-head
- sidebar

This is how a complete view can look like:

```html
<?php partial('page-head') ?>
<?php partial('sidebar') ?>

<main class="bl__main">	
	<?php partial('navigation') ?>

	<div class="bl__body container-xl">
		<h1 class="page-title">About</h1>

		// Page content
	</div>
</main>

<?php partial('page-foot') ?>
```

If we need to access the partials folder within our code, we can use the `PARTIALS` constant:

```php
echo PARTIALS; // $_SERVER['DOCUMENT_ROOT'] . APP_FOLDER . '/public/partials'
```