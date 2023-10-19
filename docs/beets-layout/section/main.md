## Structure

The main section is a wrapper for the main site components `.bl__header`, `.bl__body` and `.bl__footer`. It has no styling applied as default.

When a sidebar is used, the `.bl__main` will get an offset to the sidebar with `#!scss margin-left: $sidebar-width;`. When the sidebar is collapsed on smaller viewports, the offset is removed to fill the viewport horizontally.

<div class="image" markdown>
![title](../../assets/images/main.png)
</div>

```html
<main class="bl__main">
	<header class="bl__header">...</header>	
	<div class="bl__body">...</div>
	<footer class="bl__footer">...</footer>
</main>
```