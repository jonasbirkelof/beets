## Compiled CSS files

1. Download and unzip the compiled version of Beets Layout: **beets-layout-v1.x.x-dist.zip**.
2. Include **beets-layout.css** and **beets-layout.js** in the `<head></head>` section of your projects index file.
```html
<link rel="stylesheet" href="~/assets/css/beets-layout.css">
<script src="~/assets/js/beets-layout.js"></script>
```

## Source files

1. Download and unzip the source files: **beets-layout-v1.x.x-src.zip**.
2. Move the folders **scss/beets-layout** and **js/beets-layout** to your resource folder in your project (from where you compile your resources, i.e. **~/resources/**). You could `@include` the file **beets-layout/beets-layout.scss** in your main **app.scss** file.
3. Compile **beets-layout.scss** and **beets-layout.js** into you public assets folder (i.e. **~/public/assets/**).