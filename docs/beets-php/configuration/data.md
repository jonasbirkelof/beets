# data.php

This file is for storing app-wide static data like arrays. The file is located inside the `/src/config/` folder. 

```php title="/src/config/data.php"
$userAccountStatusCodes = [
	0 => [
		'title' => 'Inactive account',
		'short' => 'Inactive',
	],
	1 => [
		'title' => 'Active account',
		'short' => 'Active',
	],
];
```