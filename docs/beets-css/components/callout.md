<style>
.callout {
	border-radius: 6px;
	font-size: 16px;
	padding: 16px 24px;
}
.callout-header + .callout-body {
	padding-top: 8px;
}
.callout-header::before {
	margin-right: 16px;
	height: 16px;
	width: 16px;
}
.me-3 {
	margin-right: 16px !important;
}
</style>

# Callout

A callout can be used as a short information notice. Unlike the Bootstrap alert, this is not meant to call for the users attention when an action has happened. The icons are automaticlly populated within the header.

The callout has a parent class `.callout` and two child classes: `.callout-header` and `.callout-body`. 

The `.callout` class has two config classes; a styling class and an icon class.

<div class="demo">
	<div class="callout callout-note">
		<div class="callout-header">Header</div>
		<div class="callout-body">Body</div>
	</div>
	<div class="callout callout-note callout-no-icon mb-0">
		<div class="callout-header">Header</div>
		<div class="callout-body">Body</div>
	</div>
</div>
````html
<div class="callout callout-note">
	<div class="callout-header">
		Header
	</div>
	<div class="callout-body">
		Body
	</div>
</div>

<div class="callout callout-note callout-no-icon">
	<div class="callout-header">
		Header
	</div>
	<div class="callout-body">
		Body
	</div>
</div>
````

## Icons

Every callout style has a complimentary icon that is automaticlly added to the `::before` pseudo-element of `.callout-header`. The icons that are used are svg's from the free version of Font awesome.

You can remove the icon using the `.callout-no-icon` class. This makes it possible to add a custom icon.

<div class="demo">
	<div class="callout callout-note">
		<div class="callout-header">Default icon</div>
	</div>
	<div class="callout callout-note callout-no-icon">
		<div class="callout-header">No icon</div>
	</div>
	<div class="callout callout-note callout-no-icon mb-0">
		<div class="callout-header"><i class="fa-solid fa-heart me-3"></i>Custom icon</div>
	</div>
</div>
````html
<div class="callout callout-note">
	<div class="callout-header">
		Default icon
	</div>
</div>

<div class="callout callout-note callout-no-icon">
	<div class="callout-header">
		No icon
	</div>
</div>

<div class="callout callout-note callout-no-icon">
	<div class="callout-header">
		<i class="fa-solid fa-heart me-3"></i>Custom icon
	</div>
</div>
````

### Bootstrap icons

If you prefer lighter icons you can use the `.callout-bs-icons` to change out the Font awesome icons to Bootstrap icons. See the different icons under Styles below.

<div class="demo">
	<div class="callout callout-note callout-bs-icon mb-0">
		<div class="callout-header">Bootstrap icon</div>
	</div>
</div>
```html
<div class="callout callout-note callout-bs-icon">
	<div class="callout-header">
		Bootstrap icon
	</div>
</div>
```

## Styling

To style the callout you add a `.callout-{type}` class. All classes are listed below.

!!! note 
	Proper support for Boostrap theme colors will come later


=== "Default (Font awesome) icons"

	<div class="demo" markdown>
		<div class="callout callout-note mb-1">
			<div class="callout-header">Note</div>
		</div>
		<div class="callout callout-summary mb-1">
			<div class="callout-header">Summary</div>
		</div>
		<div class="callout callout-info mb-1">
			<div class="callout-header">Info</div>
		</div>
		<div class="callout callout-todo mb-1">
			<div class="callout-header">Todo</div>
		</div>
		<div class="callout callout-tip mb-1">
			<div class="callout-header">Tip</div>
		</div>
		<div class="callout callout-success mb-1">
			<div class="callout-header">Success</div>
		</div>
		<div class="callout callout-question mb-1">
			<div class="callout-header">Question</div>
		</div>
		<div class="callout callout-warning mb-1">
			<div class="callout-header">Warning</div>
		</div>
		<div class="callout callout-failure mb-1">
			<div class="callout-header">Failure</div>
		</div>
		<div class="callout callout-danger mb-1">
			<div class="callout-header">Danger</div>
		</div>
		<div class="callout callout-bug mb-1">
			<div class="callout-header">Bug</div>
		</div>
		<div class="callout callout-example mb-1">
			<div class="callout-header">Example</div>
		</div>
		<div class="callout callout-quote mb-0">
			<div class="callout-header">Quote</div>
		</div>
	</div>
	```html
	<div class="callout callout-note">...</div>
	<div class="callout callout-summary">...</div>
	<div class="callout callout-info">...</div>
	<div class="callout callout-todo">...</div>
	<div class="callout callout-tip">...</div>
	<div class="callout callout-success">...</div>
	<div class="callout callout-question">...</div>
	<div class="callout callout-warning">...</div>
	<div class="callout callout-failure">...</div>
	<div class="callout callout-danger">...</div>
	<div class="callout callout-bug">...</div>
	<div class="callout callout-example">...</div>
	<div class="callout callout-quote">...</div>
	```

=== "Bootstrap icons"

	<div class="demo" markdown>
		<div class="callout callout-note callout-bs-icon mb-1">
			<div class="callout-header">Note</div>
		</div>
		<div class="callout callout-summary callout-bs-icon mb-1">
			<div class="callout-header">Summary</div>
		</div>
		<div class="callout callout-info callout-bs-icon mb-1">
			<div class="callout-header">Info</div>
		</div>
		<div class="callout callout-todo callout-bs-icon mb-1">
			<div class="callout-header">Todo</div>
		</div>
		<div class="callout callout-tip callout-bs-icon mb-1">
			<div class="callout-header">Tip</div>
		</div>
		<div class="callout callout-success callout-bs-icon mb-1">
			<div class="callout-header">Success</div>
		</div>
		<div class="callout callout-question callout-bs-icon mb-1">
			<div class="callout-header">Question</div>
		</div>
		<div class="callout callout-warning callout-bs-icon mb-1">
			<div class="callout-header">Warning</div>
		</div>
		<div class="callout callout-failure callout-bs-icon mb-1">
			<div class="callout-header">Failure</div>
		</div>
		<div class="callout callout-danger callout-bs-icon mb-1">
			<div class="callout-header">Danger</div>
		</div>
		<div class="callout callout-bug callout-bs-icon mb-1">
			<div class="callout-header">Bug</div>
		</div>
		<div class="callout callout-example callout-bs-icon mb-1">
			<div class="callout-header">Example</div>
		</div>
		<div class="callout callout-quote callout-bs-icon mb-0">
			<div class="callout-header">Quote</div>
		</div>
	</div>
	```html
	<div class="callout callout-bs-icon callout-note">...</div>
	<div class="callout callout-bs-icon callout-summary">...</div>
	<div class="callout callout-bs-icon callout-info">...</div>
	<div class="callout callout-bs-icon callout-todo">...</div>
	<div class="callout callout-bs-icon callout-tip">...</div>
	<div class="callout callout-bs-icon callout-success">...</div>
	<div class="callout callout-bs-icon callout-question">...</div>
	<div class="callout callout-bs-icon callout-warning">...</div>
	<div class="callout callout-bs-icon callout-failure">...</div>
	<div class="callout callout-bs-icon callout-danger">...</div>
	<div class="callout callout-bs-icon callout-bug">...</div>
	<div class="callout callout-bs-icon callout-example">...</div>
	<div class="callout callout-bs-icon callout-quote">...</div>
	```

## Header

You can use the header without the body if you just want a quick info with some coloring.

<div class="demo">
	<div class="callout callout-note">
		<div class="callout-header">Header</div>
	</div>	
	<div class="callout callout-note callout-no-icon mb-0">
		<div class="callout-header">Header</div>
	</div>
</div>
````html
<div class="callout callout-note">
	<div class="callout-header">
		Header
	</div>
</div>	

<div class="callout callout-note callout-no-icon">
	<div class="callout-header">
		Header
	</div>
</div>
````

## Body

The body can contain the information you want to communicate. It can be used without the header for a more compact style.

<div class="demo">
	<div class="callout callout-note mb-0">
		<div class="callout-body">
			Body
		</div>
	</div>
</div>
````html
<div class="callout callout-note">
	<div class="callout-body">
		Body
	</div>
</div>	
````