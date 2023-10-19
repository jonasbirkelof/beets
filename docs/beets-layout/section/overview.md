The layout framework is built inside a wrapper div with `#!html id="beets-layout"`. The wrapper is required for the underlaying section classes to work.

## The structure

This is the basic structure of the framework. Most of the sections have build in classes to modify them altering functionality or rudimentary styling. 

Please refer to each sections documentation (in the left navigation) for details.

![title](../../assets/images/overview-1.png)

```html
<body>

	<div id="beets-layout">
		<aside class="bl__sidebar">			
			<div class="sidebar__header">Sidebar Header</div>
			<div class="sidebar__body">Sidebar Body</div>
			<div class="sidebar__footer">Sidebar Footer</div>
		</aside>
			
		<main class="bl__main">
			<header class="bl__header">Header</header>
			<div class="bl__body">Body</div>
			<footer class="bl__footer">Footer</footer>
		</main>
	</div>

</body>
```

First we have the wrapper that enables the classes that builds the layout, like `.bl__sidebar` and `.bl__body`. Inside the wrapper is the whole website. First we have a sidebar and then a body containing the site header, footer and the main content.

This structure makes the layout versitile since you can just remove the `#!html <aside class="bl__sidebar"></aside>` element to have a single column layout with just the body (header, main and footer) which is suitable for singe page applications.

Another good use case is when you don't nedd a lot of items in the navigation/header like links, search fields, etc. You can simply remove the `#!html <header class="bl__header"></header>` element to hide it.

## Example website

Here is a more fleshed out example of what an index page could look like with some sample php code. This example uses the Bootstrap library to get a great experience directly. You should also consider using an icon framework like [Font Awesome](https://fontawesome.com/) for your sidebar/header toggle buttons and sidebar links instead of the basic characters used in this example.

Please refer to each sections documentation (in the left navigation) for details.

```php title="functions.php"
<?php

function pageIsActive($uris) 
{
	$pageIsActive = false;
	$page = isset($_GET['page']) ? $_GET['page'] : null;

	foreach ($uris as $uri) {
		if ($page == $uri) $pageIsActive = true;
	}

	return $pageIsActive;
}

function activePage($uris) 
{
	return pageIsActive($uris) ? 'active' : null;
}

function openSubmenu($uris) 
{
	return pageIsActive($uris) ? 'open' : null;
}
```

```html title="index.php"
<?php

require "functions.php";

?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Beets Layout</title>
	<!-- Link to your Bootstrap files -->
    <link rel="stylesheet" href="bootstrap.css">
    <script src="bootstrap.bundle.js"></script>
	<!-- Link to your Beets Layout files -->
    <link rel="stylesheet" href="beets-layout.css">
    <script src="beets-layout.js"></script>
</head>
<body>

    <!-- Beets Layout wrapper -->
	<div id="beets-layout">
		
		<!-- Sidebar -->
		<aside class="bl__sidebar sidebar-collapse sidebar-dark bg-dark">

			<div class="sidebar__header justify-content-between">
				<div class="header__logo">
					<img src="logotype.png" alt="Logotype">
				</div>
				<button class="btn btn-primary d-lg-none" onclick="toggleSidebar()">X</button>
			</div>

			<div class="sidebar__body">
				<!-- Navigation -->
				<nav class="sidebar__nav nav-rounded nav-accent-primary">
					<ul class="nav-list mb-0">
						<li class="list-item">
							<a href="?page=dashboard" class="nav-link <?= activePage(['dashboard']) ?>">
								<div class="nav-link-icon">&lt;i&gt;</div>
								Dashboard
							</a>
						</li>
						<?php $submenuItems = ['addUser', 'listUsers', 'removeUser']; ?>
						<li class="list-item <?= openSubmenu($submenuItems) ?>">
							<a href="#" class="nav-link nav-link__submenu <?= activePage($submenuItems) ?>" id="sub_users" onclick="toggleSubmenu('sub_users')">
								<div class="nav-link-icon">&lt;i&gt;</div>
								Users
							</a>
							<ul class="nav-list__submenu">
								<li class="list-item"><a href="?page=addUser" class="nav-link <?= activePage(['addUser']) ?>">Add user</a></li>
								<li class="list-item"><a href="?page=listUsers" class="nav-link <?= activePage(['listUsers']) ?>">List users</a></li>
								<li class="list-item"><a href="?page=removeUser" class="nav-link <?= activePage(['removeUser']) ?>">Remove user</a></li>
							</ul>
						</li>
						<li class="list-item">
							<a href="?page=settings" class="nav-link <?= activePage(['settings']) ?>">
								<div class="nav-link-icon">&lt;i&gt;</div>
								Settings
							</a>
						</li>
					</ul>
				</nav>

				<hr class="sidebar__divider">

				<nav class="sidebar__nav nav-compact nav-accent-primary">
					<ul class="nav-list">
						<li class="list-item">
							<a href="?page=link-1" class="nav-link <?= activePage(['link-1']) ?>">Link 1</a>
						</li>
						<li class="list-item">
							<a href="?page=link-2" class="nav-link <?= activePage(['link-2']) ?>">Link 2</a>
						</li>
						<li class="list-item">
							<a href="?page=link-3" class="nav-link <?= activePage(['link-3']) ?>">Link 3</a>
						</li>
					</ul>
				</nav>

			</div>

			<div class="sidebar__footer">
				SIDEBAR FOOTER
			</div>

		</aside>
			
		<!-- Main -->
		<main class="bl__main">

			<!-- Header -->
			<header class="bl__header justify-content-between header-sticky px-3 px-lg-4">
				
				<button class="btn btn-primary d-lg-none" onclick="toggleSidebar()">=</button>
				<div class="header__logo d-lg-none">
					<img src="logotype.png" alt="Logotype">
				</div>
				<button class="btn btn-primary d-lg-none" onclick="toggleHeader('header-nav')">...</button>

				<div class="header-collapse" id="header-nav">
					<nav class="header__nav">
						<ul class="nav-list">
							<li class="nav-item"><a href="?page=home" class="nav-link <?= activePage(['home']) ?>">Home</a></li>
							<li class="nav-item"><a href="?page=about" class="nav-link <?= activePage(['about']) ?>">About Us</a></li>
							<li class="nav-item"><a href="?page=contact" class="nav-link <?= activePage(['contact']) ?>">Contact</a></li>
						</ul>
					</nav>
				</div>

			</header>

			<!-- Body -->
			<div class="bl__body">
				<div class="container-fluid px-3 px-lg-4 pt-2">
					BODY
				</div>
			</div>		
			
			<!-- Footer -->
			<footer class="bl__footer">
				FOOTER
			</footer>

		</main>

	</div>

</body>
</html>
```