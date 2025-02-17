There are some thing to know before deploying you Beets PHP application. In the best of worlds you want to apply the best security practises possible, but sometimes that is just not possible.

This page will present some of your options so htat you candecide how you want to go by deploying the Beets PHP application based on the limitations on your host.

## Alternative 1 - Best practise

The best practise is to seperate you backend and frontend on your host. For example:

<div class="file-tree">
<pre>
/ (website root)
&#9500;&#9472;&#9472; httpd_public/ or www/
&#9474;   &#9492;&#9472;&#9472; [frontend files]
&#9492;&#9472;&#9472; [backend files]
</pre>
</div>

With Beets PHP, your frontend files are the ones in the `/src/public/` folder as well as the main `index.php` file in the project root. The backend files are all other files and folders.

You want to do this to make you important files, like the `.env` file with your database credentials, API keys, etc inaccessible to your visitors. The visitors will only have access to the public folder that your webhost has set up. It can be named `www`, `httpd_public`, `public`, `http` or something like that.

There is no need to upload the `node_modules` folder or the `resources` folder.

### The index.php file

The main `index.php` file has to be in the "root" in the public folder for the router wo work so depending on how your project is set up you might have to move it in there.

You will also have to update the file paths in the `index.php` file to match the new file structure.

### The .env file

Upload the `.env.production` file and rename it to `.env`. This file don't have to be re-uploaded unless there have been any changes to it, so you can add it to your `.gitignore` file. Do not upload the `.env` file that is used for local developement! 

### File structure example

Below is an example of a file structure that should work for this situation. Just note that it has not been officially tested by me since I myself don't have access to this kind of web host.

<div class="file-tree">
<pre>
/
&#9500;&#9472;&#9472; httpd_public/
&#9474;   &#9500;&#9472;&#9472; assets/
&#9474;   &#9500;&#9472;&#9472; partials/
&#9474;   &#9500;&#9472;&#9472; views/
&#9474;   &#9492;&#9472;&#9472; index.php
&#9500;&#9472;&#9472; App/
&#9500;&#9472;&#9472; config/
&#9500;&#9472;&#9472; routes/
&#9500;&#9472;&#9472; vendor/
&#9500;&#9472;&#9472; .env
&#9500;&#9472;&#9472; .htaccess
&#9500;&#9472;&#9472; composer.json
&#9500;&#9472;&#9472; composer.lock
&#9500;&#9472;&#9472; LICENSE
&#9500;&#9472;&#9472; package-lock.json
&#9492;&#9472;&#9472; package.json
</pre>
</div>

## Alternative 2 - Using only the public folder

Sometimes you don't have permissions to use the root folder on you website, for instance whe you have a "shared host". This is decided by your host and if you can't change host to use best prcatise, you can work around this to make the best of the situation.

Upload all of the project files to the public folder on your host. It is the top most directory that you have writing persmissions for and it can be called `www`, `httpd_public`, `public`, `http` or something like that.

By following the steps for setting up Beets PHP for Xampp you have kind of already prepared your application for this scenario.

There is no need to upload the `node_modules` folder or the `resources` folder.

### The index.php file

The main `index.php` file has to be in the "root" in the public folder for the router wo work.

### The .env file

Upload the `.env.production` file and rename it to `.env`. This file don't have to be re-uploaded unless there have been any changes to it, so you can add it to your `.gitignore` file. Do not upload the `.env` file that is used for local developement! 

### File structure example

Below is an example of a file structure that should work for this situation.

<div class="file-tree">
<pre>
/
&#9492;&#9472;&#9472; httpd_public/
    &#9500;&#9472;&#9472; App/
    &#9500;&#9472;&#9472; config/
    &#9500;&#9472;&#9472; public/
    &#9474;   &#9500;&#9472;&#9472; assets/
    &#9474;   &#9500;&#9472;&#9472; partials/
    &#9474;   &#9492;&#9472;&#9472; views/
    &#9500;&#9472;&#9472; routes/
    &#9500;&#9472;&#9472; vendor/
    &#9500;&#9472;&#9472; .env
    &#9500;&#9472;&#9472; .htaccess
    &#9500;&#9472;&#9472; composer.json
    &#9500;&#9472;&#9472; composer.lock
    &#9500;&#9472;&#9472; index.php
    &#9500;&#9472;&#9472; LICENSE
    &#9500;&#9472;&#9472; package-lock.json
    &#9492;&#9472;&#9472; package.json
</pre>
</div>

### Application as subdomain

If you application is deployed as a subdomain like **app.example.com** instead of **example.com**, you might have to specify that in the `/src/config/config.php` file. Update the variable `$appFolder` the the subfolder name. This will change the project root that is used for loading files and paths through out the application.

Below is anexample of a file structure when the application is a subdomain.

<div class="file-tree">
<pre>
/
&#9492;&#9472;&#9472; httpd_public/
    &#9492;&#9472;&#9472; subdomain/
        &#9500;&#9472;&#9472; App/
        &#9500;&#9472;&#9472; config/
        &#9500;&#9472;&#9472; public/
        &#9500;&#9472;&#9472; routes/
        &#9500;&#9472;&#9472; vendor/
        &#9492;&#9472;&#9472; ...
</pre>
</div>

The config file might then have to look like this:

```php
// Production variables
if (APP_ENV == 'production') {
	$appFolder = '/subdomain';
}
```