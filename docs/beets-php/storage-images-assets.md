The `/src/public` folder in Beets PHP contains files that should be accessible for the user, like our views and assets like images. We have three folders that contians these assets: 

- `/storage`
- `/images`
- `/assets`

## Storage

The storage folder is located in `/src/public` and contains all of the files uploaded through your application. One example is user profile pictures that the users can upload from the Profile page.

To get a file from within the storage folder, we can use the `storage()` helper function. Pass the file name as a string:

```html
<div class="profile-image__container">
	<img src="<?= storage('user-image.jpg') ?>" class="profile-image">									
</div>
```

If we need to access the storage folder within our code, we can use the `STORAGE` constant:

```php
echo STORAGE; // $_SERVER['DOCUMENT_ROOT'] . APP_FOLDER . '/public/storage'
```

## Images

The images folder is located in `/src/public` and contains images that the application uses, like logos and graphics.

To get a file from within the images folder, we can use the `image()` helper function. Pass the file name as a string:

```html
<div class="header__logo">
	<img src="<?= image('beets_col_250x744.png') ?>">
</div>
```

If we need to access the images folder within our code, we can use the `IMAGES` constant:

```php
echo IMAGES; // $_SERVER['DOCUMENT_ROOT'] . APP_FOLDER . '/public/images'
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <link rel="shortcut icon" href="<?= IMAGES ?>/favicon.ico" type="image/x-icon">
</head>
```

## Assets

The assets folder is located in `/src/public` and contains the compiled CSS files and JavaScript files.

To get a file from within the assets folder, we can use the `ASSETS` constant and concatinate the `css` or `js` folder followed by the file name, but more convenient is to use the `CSS` or `JAVASCRIPT` constants that include the whole path to that directory.

```php
echo ASSETS; // $_SERVER['DOCUMENT_ROOT'] . APP_FOLDER . '/public/assets'

echo CSS; // $_SERVER['DOCUMENT_ROOT'] . APP_FOLDER . '/public/assets/css'
echo JAVASCRIPT; // $_SERVER['DOCUMENT_ROOT'] . APP_FOLDER . '/public/assets/js'
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <script src="<?= JAVASCRIPT ?>/beets-layout.js"></script>
    <link rel="stylesheet" href="<?= CSS ?>/app.css">
</head>
```