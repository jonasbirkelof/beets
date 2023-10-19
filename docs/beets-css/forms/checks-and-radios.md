# Checks and Radios

In Beets CSS you have assex to checkboxes, radios and switches styled by the theme colors. The reason why Bootstrap haven't included this functionality is propably because they have build in form validation, that styles the form controls like `success` or `danger` if the input is valid or invalid. This is a really good reason so be careful styling your forms with the classes below!

What is actually useful is the fact that you can create [your own theme color](../../utilities/colors) and use it within your form! 

You add the `.form-check-*` class to any `<div class="form-check">` to style the form control.

## Checkbox

<div class="demo">
	<div class="form-check form-check-inline form-check-primary">
		<input class="form-check-input" type="checkbox" id="inlineCheckbox1" checked>
		<label class="form-check-label" for="inlineCheckbox1">Primary</label>
	</div>
	<div class="form-check form-check-inline form-check-secondary">
		<input class="form-check-input" type="checkbox" id="inlineCheckbox2" checked>
		<label class="form-check-label" for="inlineCheckbox2">Secondary</label>
	</div>
	<div class="form-check form-check-inline form-check-success">
		<input class="form-check-input" type="checkbox" id="inlineCheckbox3" checked>
		<label class="form-check-label" for="inlineCheckbox3">Success</label>
	</div>
	<div class="form-check form-check-inline form-check-warning">
		<input class="form-check-input" type="checkbox" id="inlineCheckbox4" checked>
		<label class="form-check-label" for="inlineCheckbox4">Warning</label>
	</div>
	<br>
	<div class="form-check form-check-inline form-check-danger">
		<input class="form-check-input" type="checkbox" id="inlineCheckbox5" checked>
		<label class="form-check-label" for="inlineCheckbox5">Danger</label>
	</div>
	<div class="form-check form-check-inline form-check-info">
		<input class="form-check-input" type="checkbox" id="inlineCheckbox6" checked>
		<label class="form-check-label" for="inlineCheckbox6">Info</label>
	</div>
	<div class="form-check form-check-inline form-check-light">
		<input class="form-check-input" type="checkbox" id="inlineCheckbox7" checked>
		<label class="form-check-label" for="inlineCheckbox7">Light</label>
	</div>
	<div class="form-check form-check-inline form-check-dark">
		<input class="form-check-input" type="checkbox" id="inlineCheckbox8" checked>
		<label class="form-check-label" for="inlineCheckbox8">Dark</label>
	</div>
</div>
```html
<div class="form-check form-check-inline form-check-primary">
	<input class="form-check-input" type="checkbox" id="inlineCheckbox1">
	<label class="form-check-label" for="inlineCheckbox1">Primary</label>
</div>
```

## Radio

<div class="demo">
	<div class="form-check form-check-inline form-check-primary">
		<input class="form-check-input" type="radio" id="inlineRadio1" name="inlineRadio1" checked>
		<label class="form-check-label" for="inlineRadio1">Primary</label>
	</div>
	<div class="form-check form-check-inline form-check-secondary">
		<input class="form-check-input" type="radio" id="inlineRadio2" name="inlineRadio2" checked>
		<label class="form-check-label" for="inlineRadio2">Secondary</label>
	</div>
	<div class="form-check form-check-inline form-check-success">
		<input class="form-check-input" type="radio" id="inlineRadio3" name="inlineRadio3" checked>
		<label class="form-check-label" for="inlineRadio3">Success</label>
	</div>
	<div class="form-check form-check-inline form-check-warning">
		<input class="form-check-input" type="radio" id="inlineRadio4" name="inlineRadio4" checked>
		<label class="form-check-label" for="inlineRadio4">Warning</label>
	</div>
	<br>
	<div class="form-check form-check-inline form-check-danger">
		<input class="form-check-input" type="radio" id="inlineRadio5" name="inlineRadio5" checked>
		<label class="form-check-label" for="inlineRadio5">Danger</label>
	</div>
	<div class="form-check form-check-inline form-check-info">
		<input class="form-check-input" type="radio" id="inlineRadio6" name="inlineRadio6" checked>
		<label class="form-check-label" for="inlineRadio6">Info</label>
	</div>
	<div class="form-check form-check-inline form-check-light">
		<input class="form-check-input" type="radio" id="inlineRadio7" name="inlineRadio7" checked>
		<label class="form-check-label" for="inlineRadio7">Light</label>
	</div>
	<div class="form-check form-check-inline form-check-dark mb-0">
		<input class="form-check-input" type="radio" id="inlineRadio8" name="inlineRadio8" checked>
		<label class="form-check-label" for="inlineRadio8">Dark</label>
	</div>
</div>
```html
<div class="form-check form-check-inline form-check-primary">
	<input class="form-check-input" type="radio" id="inlineRadio1" name="inlineRadio1">
	<label class="form-check-label" for="inlineRadio1">Primary</label>
</div>
```

## Switch

<div class="demo">
	<div class="form-check form-switch form-check-inline form-check-primary">
		<input class="form-check-input" type="checkbox" id="inlineCheckbox1" checked>
		<label class="form-check-label" for="inlineCheckbox1">Primary</label>
	</div>
	<div class="form-check form-switch form-check-inline form-check-secondary">
		<input class="form-check-input" type="checkbox" id="inlineCheckbox2" checked>
		<label class="form-check-label" for="inlineCheckbox2">Secondary</label>
	</div>
	<div class="form-check form-switch form-check-inline form-check-success">
		<input class="form-check-input" type="checkbox" id="inlineCheckbox3" checked>
		<label class="form-check-label" for="inlineCheckbox3">Success</label>
	</div>
	<div class="form-check form-switch form-check-inline form-check-warning">
		<input class="form-check-input" type="checkbox" id="inlineCheckbox4" checked>
		<label class="form-check-label" for="inlineCheckbox4">Warning</label>
	</div>
	<br>
	<div class="form-check form-switch form-check-inline form-check-danger">
		<input class="form-check-input" type="checkbox" id="inlineCheckbox5" checked>
		<label class="form-check-label" for="inlineCheckbox5">Danger</label>
	</div>
	<div class="form-check form-switch form-check-inline form-check-info">
		<input class="form-check-input" type="checkbox" id="inlineCheckbox6" checked>
		<label class="form-check-label" for="inlineCheckbox6">Info</label>
	</div>
	<div class="form-check form-switch form-check-inline form-check-light">
		<input class="form-check-input" type="checkbox" id="inlineCheckbox7" checked>
		<label class="form-check-label" for="inlineCheckbox7">Light</label>
	</div>
	<div class="form-check form-switch form-check-inline form-check-dark">
		<input class="form-check-input" type="checkbox" id="inlineCheckbox8" checked>
		<label class="form-check-label" for="inlineCheckbox8">Dark</label>
	</div>
</div>
```html
<div class="form-check form-switch form-check-inline form-check-primary">
	<input class="form-check-input" type="checkbox" id="inlineCheckbox1">
	<label class="form-check-label" for="inlineCheckbox1">Primary</label>
</div>
```