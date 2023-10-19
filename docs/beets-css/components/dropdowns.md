<style>
.md-typeset .demo ul {
	list-style-type: none;
	margin-bottom: 0;
	margin-left: 0;
	margin-top: 3px;
	padding: 8px 0;
}
.md-typeset .demo ul li {
	margin: 0;
}
.demo .dropdown-menu {
	border-radius: 6px;
	display: flex;
	flex-shrink: 1;
	min-width: 160px;
	position: initial;
	width: 178px;
}
.demo .dropdown-item {
	color: var(--bs-body-color);
	font-size: 16px;
	padding: 4px 16px;
}
.demo .dropdown-item,
.demo .dropdown-item:hover {
	color: var(--bs-body-color) !important;
}
</style>

# Dropdowns

The Beets CSS dropdowns has a nice shadow!

<div class="demo">
	<div class="dropdown">
		<button class="btn btn-primary dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false">
			Dropdown button
		</button>
		<ul class="dropdown-menu">
			<li><a class="dropdown-item" href="#">Action</a></li>
			<li><a class="dropdown-item" href="#">Another action</a></li>
			<li><a class="dropdown-item" href="#">Something else here</a></li>
		</ul>
	</div>
</div>
```html
<div class="dropdown">
	<button class="btn btn-primary dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false">
		Dropdown button
	</button>
	<ul class="dropdown-menu">
		<li><a class="dropdown-item" href="#">Action</a></li>
		<li><a class="dropdown-item" href="#">Another action</a></li>
		<li><a class="dropdown-item" href="#">Something else here</a></li>
	</ul>
</div>
```

## No caret

With the `.dropdown-no-caret` class on the `<button>` tag you can remove the default caret from the dropdown button. This is really useful when you want a nice menu button, for instance.

<div class="demo">
	<div class="btn-group m-1">
		<div class="dropdown">
			<button class="btn btn-primary dropdown-toggle dropdown-no-caret" type="button" data-bs-toggle="dropdown" aria-expanded="false">
				No caret
			</button>
		</div>
	</div>
	<div class="btn-group m-1">
		<div class="dropdown">
			<button class="btn btn-primary btn-square dropdown-toggle dropdown-no-caret" type="button" data-bs-toggle="dropdown" aria-expanded="false">
				<i class="fa-solid fa-bars"></i>
			</button>
		</div>
	</div>
	<div class="btn-group m-1">
		<div class="dropdown">
			<button class="btn btn-ghost-secondary btn-square rounded-circle dropdown-toggle dropdown-no-caret" type="button" data-bs-toggle="dropdown" aria-expanded="false">
				<i class="fa-solid fa-cog"></i>
			</button>
		</div>
	</div>
</div>
```html
<div class="dropdown">
	<button class="btn btn-primary dropdown-toggle dropdown-no-caret" type="button" data-bs-toggle="dropdown" aria-expanded="false">
		No caret
	</button>
	...
</div>
<div class="dropdown">
	<button class="btn btn-primary btn-square dropdown-toggle dropdown-no-caret" type="button" data-bs-toggle="dropdown" aria-expanded="false">
		<i class="fa-solid fa-bars"></i>
	</button>
	...
</div>
<div class="dropdown">
	<button class="btn btn-ghost-secondary btn-square rounded-circle dropdown-toggle dropdown-no-caret" type="button" data-bs-toggle="dropdown" aria-expanded="false">
		<i class="fa-solid fa-cog"></i>
	</button>
	...
</div>
```

### Custom caret

With the default caret removed, you can easily add your own custom carets with something like Font Awesome and som styling!

<div class="demo">
	<div class="dropdown">
		<button class="btn btn-primary dropdown-toggle dropdown-no-caret" type="button" data-bs-toggle="dropdown" aria-expanded="false">
			Custom caret
		<i class="fa-solid fa-xs fa-chevron-down ms-1"></i>
		</button>
	</div>
</div>
```html hl_lines="4"
<div class="dropdown">
	<button class="btn btn-primary dropdown-toggle dropdown-no-caret" type="button" data-bs-toggle="dropdown" aria-expanded="false">
		Custom caret
		<i class="fa-solid fa-xs fa-chevron-down ms-1"></i>
	</button>
	<ul class="dropdown-menu">
		<li><a class="dropdown-item" href="#">Action</a></li>
		<li><a class="dropdown-item" href="#">Another action</a></li>
		<li><a class="dropdown-item" href="#">Something else here</a></li>
	</ul>
</div>
```