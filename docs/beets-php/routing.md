Beets PHP uses the [Bramus Router](https://github.com/bramus/router) for routing HTTP requests. The router is required in the main `~/index.php` file and is always loaded. 

## web.php

```php title="Location"
~/routes/web.php
```

The router file is populated with several routes out of the box so that you have good examples of how to use it. For detailed instructions on how to use the router, please visit the [GitHub page](https://github.com/bramus/router).

## Custom request methods

By default the Bramus Router doeas not have support for all of the request methods. This is solved for Beets PHP as one of the installation step when we change one line of code in the router's source code. The updated code will check and see if `$_POST['_method']` is set and use that, but of it's not set get the request method as usual. We want to do this so that we can set a request method like `PATCH` for instance and we have the `method()` helper function to easily do this.

For instructions on how to use the `method()` function, please visit the [Form functions page](form-functions.md#form-method).