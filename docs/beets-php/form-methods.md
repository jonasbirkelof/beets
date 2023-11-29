# Form methods

The HTML protocol can only send and recieve POST and GET request methods from a `<form>` tag. Our RESTful framework makes use of the PATCH and DELETE methods so we can use the `method()` helper function to make this work.

The `method()` function is located in the [application functions file](./app-functions.md) `~/app/core/functions.php`.

The `method()` function will return an input field with the necessary properties as well as the request method of your chosing. When the router then runs the request it will look for a `#!php $_POST['_method']` and use that as the request method instead of POST that was really sent.

```php
echo method('PATCH');
```

Output:

```html
<input type="hidden" name="_method" value="PATCH">
```

In a form in a view it might look something like this:

```html
<form method="POST" name="my-form" id="myForm">
	<?= method('PATCH') ?>
	<label for="name">Name</label>
	<input type="text" name="name">
</form>
```