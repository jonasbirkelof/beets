<!-- Styling for buttons in custom.css -->

# Buttons

Beets CSS comes with two additional button styles: **Ghost** and **Alternative**. There is also a class that forces the button to be an exact square. Useful for buttons containing icons or user initials.

## Styles

The Beets CSS styles both make use of the Bootstrap theme colors for your convenience.

### Regular

!!! note
	This is the standard Bootstrap button. This is in the documentation for comparison purposes.

`.btn-{theme-color}`

<div class="demo">
	<button class="btn btn-primary">Primary</button>
	<button class="btn btn-secondary">Secondary</button>
	<button class="btn btn-success">Success</button>
	<button class="btn btn-warning">Warning</button>
	<button class="btn btn-danger">Danger</button>
	<button class="btn btn-info">Info</button>
	<button class="btn btn-light">Light</button>
	<button class="btn btn-dark">Dark</button>
	<button type="button" class="btn btn-link">Link</button>
</div>

````html
<button class="btn btn-primary">Primary</button>
<button class="btn btn-link">Link</button>
````

### Outline

!!! note
	This is the standard outlined Bootstrap button. This is in the documentation for comparison purposes.

`.btn-outline-{theme-color}`

<div class="demo">
	<button class="btn btn-outline-primary">Primary</button>
	<button class="btn btn-outline-secondary">Secondary</button>
	<button class="btn btn-outline-success">Success</button>
	<button class="btn btn-outline-warning">Warning</button>
	<button class="btn btn-outline-danger">Danger</button>
	<button class="btn btn-outline-info">Info</button>
	<button class="btn btn-outline-light">Light</button>
	<button class="btn btn-outline-dark">Dark</button>
</div>

````html
<button class="btn btn-outline-primary">Primary</button>
````

### Alternative

This style can be viewed as a slightly lighter version of the standard buttons.

`.btn-alt-{theme-color}`

<div class="demo">
	<button class="btn btn-alt-primary">Primary</button>
	<button class="btn btn-alt-secondary">Secondary</button>
	<button class="btn btn-alt-success">Success</button>
	<button class="btn btn-alt-warning">Warning</button>
	<button class="btn btn-alt-danger">Danger</button>
	<button class="btn btn-alt-info">Info</button>
	<button class="btn btn-alt-light">Light</button>
	<button class="btn btn-alt-dark">Dark</button>
</div>

````html
<button class="btn btn-alt-primary">Primary</button>
<button class="btn btn-alt-secondary">Secondary</button>
<button class="btn btn-alt-success">Success</button>
<button class="btn btn-alt-warning">Warning</button>
<button class="btn btn-alt-danger">Danger</button>
<button class="btn btn-alt-info">Info</button>
<button class="btn btn-alt-light">Light</button>
<button class="btn btn-alt-dark">Dark</button>
````

### Ghost

This style makes the button background transparent with the text colored with the chosen theme color. When hovering, the background gets a light version of the theme color.

`.btn-ghost-{theme-color}`

<div class="demo">
	<button class="btn btn-ghost-primary">Primary</button>
	<button class="btn btn-ghost-secondary">Secondary</button>
	<button class="btn btn-ghost-success">Success</button>
	<button class="btn btn-ghost-warning">Warning</button>
	<button class="btn btn-ghost-danger">Danger</button>
	<button class="btn btn-ghost-info">Info</button>
	<button class="btn btn-ghost-light">Light</button>
	<button class="btn btn-ghost-dark">Dark</button>
</div>

````html
<button class="btn btn-ghost-primary">Primary</button>
<button class="btn btn-ghost-secondary">Secondary</button>
<button class="btn btn-ghost-success">Success</button>
<button class="btn btn-ghost-warning">Warning</button>
<button class="btn btn-ghost-danger">Danger</button>
<button class="btn btn-ghost-info">Info</button>
<button class="btn btn-ghost-light">Light</button>
<button class="btn btn-ghost-dark">Dark</button>
````

## Square

This is designed for use with an icon like FontAwsome, or short text like initials.
The width and height are the same and the content is center vertically and horizontally.

To use the square button style, add `.btn-square` to your standard `.btn` button.

### Sizes

The `.btn-square` class also supports the different button sizes.

<div class="demo">
	<button class="btn btn-primary btn-square btn-lg me-1"><i class="fa-solid fa-user fa-lg"></i></button>
	<button class="btn btn-primary btn-square me-1"><i class="fa-solid fa-user"></i></button>
	<button class="btn btn-primary btn-square btn-sm"><i class="fa-solid fa-user fa-sm"></i></button>
</div>

````html
<button class="btn btn-primary btn-square btn-lg"><i class="fa-solid fa-user fa-lg"></i></button>
<button class="btn btn-primary btn-square"><i class="fa-solid fa-user"></i></button>
<button class="btn btn-primary btn-square btn-sm"><i class="fa-solid fa-user fa-sm"></i></button>
````

### Inspiration

Here are some buttons for your inspiration!

<div class="demo">
	<button class="btn btn-square btn-primary me-1"><i class="fa-solid fa-user"></i></button>
	<button class="btn btn-square btn-primary rounded-circle me-1">JB</button>
	<button class="btn btn-square btn-outline-secondary rounded-circle me-1"><i class="fa-solid fa-pen"></i></button>
	<button class="btn btn-square btn-outline-danger rounded-0 me-1"><i class="fa-solid fa-trash"></i></button>
	<button class="btn btn-square btn-ghost-secondary me-1"><i class="fa-solid fa-bars"></i></button>
	<button class="btn btn-square btn-ghost-dark rounded-circle"><i class="fa-solid fa-arrow-left"></i></button>
</div>

````html
<button class="btn btn-square btn-primary"><i class="fa-solid fa-user"></i></button>
<button class="btn btn-square btn-primary rounded-circle">JB</button>
<button class="btn btn-square btn-outline-secondary rounded-circle"><i class="fa-solid fa-pen"></i></button>
<button class="btn btn-square btn-outline-danger rounded-0"><i class="fa-solid fa-trash"></i></button>
<button class="btn btn-square btn-ghost-secondary"><i class="fa-solid fa-bars"></i></button>
<button class="btn btn-square btn-ghost-dark rounded-circle"><i class="fa-solid fa-arrow-left"></i></button>
````
### Responsiveness

You can use breakpoints `.btn-square-{sm|md|lg|xl|xxl}-0` to decide from what screen size a button should have the square class.

<div class="demo">
	<button class="btn btn-primary btn-square me-1">XS</button>
	<button class="btn btn-primary btn-square-sm me-1">SM</button>
	<button class="btn btn-primary btn-square-md me-1">MD</button>
	<button class="btn btn-primary btn-square-lg me-1">LG</button>
	<button class="btn btn-primary btn-square-xl me-1">XL</button>
	<button class="btn btn-primary btn-square-xxl">XXL</button>
</div>

````html
<button class="btn btn-primary btn-square">XS</button>
<button class="btn btn-primary btn-square-sm">SM</button>
<button class="btn btn-primary btn-square-md">MD</button>
<button class="btn btn-primary btn-square-lg">LG</button>
<button class="btn btn-primary btn-square-xl">XL</button>
<button class="btn btn-primary btn-square-xxl">XXL</button>
````

<!-- |                 | Activates on screen             |            |
|-----------------|:--------------------------------|------------|
| .btn-square     | Extra small (every screen size) | <576px     |
| .btn-square-sm  | Small                           | &ge;576px  |
| .btn-square-md  | Meduim                          | &ge;768px  |
| .btn-square-lg  | Large                           | &ge;992px  |
| .btn-square-xl  | Extra Large                     | &ge;1200px |
| .btn-square-xxl | Extra extra large               | &ge;1400px | -->

#### Reset

You can also use the reset class `.btn-square-0` to remove the square styling.

The reset class is also responsive with the `.btn-square-{sm|md|lg|xl|xxl}-0` class which makes it useful for when you don't want to have a square button on smaller screen sizes.

<div class="demo">
	<button class="btn btn-primary btn-square-0 me-1">XS</button>
	<button class="btn btn-primary btn-square-sm-0 me-1">SM</button>
	<button class="btn btn-primary btn-square-md-0 me-1">MD</button>
	<button class="btn btn-primary btn-square-lg-0 me-1">LG</button>
	<button class="btn btn-primary btn-square-xl-0 me-1">XL</button>
	<button class="btn btn-primary btn-square-xxl-0">XXL</button>
</div>

````html
<button class="btn btn-primary btn-square-0">XS</button>
<button class="btn btn-primary btn-square-sm-0">SM</button>
<button class="btn btn-primary btn-square-md-0">MD</button>
<button class="btn btn-primary btn-square-lg-0">LG</button>
<button class="btn btn-primary btn-square-xl-0">XL</button>
<button class="btn btn-primary btn-square-xxl-0">XXL</button>
````