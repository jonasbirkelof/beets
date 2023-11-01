# Sessions

With Beets PHP you get a convenient session handling. With the built in methods you can easily set, get and reset custom keys. With a flash session, you can create values that is flushed (unset) automatically and directly after a page load. This is perfect for alerts!

All session methods belong to the Session class in the file `~/app/core/Session.php`. For details, please se the [Session.php](./classes/Session.md) page.

## About flash messages

A flash message is used to temporarely show a message and then immediately delete it. A common use case is when you need to show an alert when a task was successful (or unsuccessful), like "The post has been created".

With Beets PHP, the flash messages is automatically deleted (flushed) when the index page (~/public/index.php) has been loaded, right after the massage has been showed.

The flash messages is stored in the global session array with the key `_flash`, like this:

```php
$_SESSION["_flash"][$key][$value];
```

## Set a flash message

Setting a flash message is really easy with the provided [`flash()`](./classes/Session.md#flash) method:

```php
Session::flash("message", "This is a flash message!");
```

This code will set the `$key` to "message" and the `$value` to "This is a flash message!". You can name the key anything you want that makes sense for the nature of the message. For instance "error" if you are setting an error message.

## Get the flash message

When the flash message has been set, you can get it by calling the [`get()`](./classes/Session.md#get) method like this:

```php
Session::flash("message", "This is a flash message!");

echo Session::get("message"); // This is a flash message!
```

The `get()` method recieves two parameters, the key and the optional fallback value. The method will first check if the given key exists in the _flash array (`$_SESSION[_flash][$key]`). Then, if the key does not exists in the _flash array, it will check in the global session (`$_SESSION[$key]`). If the key exists i neither of these arrays, the method will return the fallback value which is `null` by default. 

The code below will demonstrate what will happen if the given key does not exist in the session.

```php
Session::flash("message", "This is a message");

echo Session::get("msg", "Error"); // Error
```

This code will echo out "Error" since the key "msg" was not set in the flash array ("message" was) and it does not exist in the global session array (`$_SESSION["msg"]`).

## Reset the flash message

The flash message is automatically unflashed (unset or reset) at the end of the page load. However, if you want to manually unflash the session, you can do it with the with the [`unflash()`](./classes/Session.md#unflash) method:

```php
Session::unflash();
```

The `unflash()` method does not return anything (return type: void) and does not  accept any parameters.

## Custom sessions

If you don't want your session to be reset after every page load like the flash messages does, you can set you own keys to the global session variable with the [`put()`](./classes/Session.md#put) method:

```php
Session::put("myName", "Dwight"); // $_SESSION["myName"] = "Dwight"
```

To get the session variable you use the same method as with the flash messages, the [`get()`](./classes/Session.md#get) method:

```php
echo Session::get("myName"); // Dwight
```

Just like when getting flash messages, you can insert a fallback value in case the key you are looking for does not exist in the session. The code below will echo out "Error" since the key "name" does not exist in the session.

```php
Session::put("myName", "Dwight");

echo Session::get("name", "Error"); // Error
```

To remove the value and key from the session you use the method [`flush()`](./classes/Session.md#flush) which you can read more about [below](#empty-the-session).

## Check if a key exists in the session

There is a practical method you can use if you just want to verify if a specific key exists in the session. Use the [`has()`](./classes/Session.md#has) method and insert the key as a parameter. The method will only return a boolean and it makes use of the `get()` method for checking, so this works with both custom session variables and flash messages.

```php
Session::put("myKey", "myValue");

echo Session::has("myKey") ? "true" : "false"; // true
```

## Empty the session

You can empty the whole session by setting it to an empty array, or you can unset only a given key with the [`flush()`](./classes/Session.md#flush) method.

To empty the whole session, you leave the paramaters empty.

```php
Session::flush(); // Empty the whole session
Session::flush("myName") // Empty only the key "myName"
```

If a parameter (key) is set, the key will be unset (`unset($_SESSION[$key])`) instead of being set to an empty array.

## Destroying the session

The [`destroy()`](./classes/Session.md#destroy) method will destroy the whole session, including the logged in user credentials. It will flush the session, destroy it and reset the coockie parameters. This method is called when a user is logging out.

The method is not returning anything (return type: void) and does not accept any parameters. To execute it, you just call it:

```php
function logout() {
	Session::destroy(); // Destroy the session
}
```