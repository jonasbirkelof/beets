# Colors

If you use a sass compiler you can easily add your own custom colors to the css variables and Bootstrap theme. This can be done even without Beets CSS, but we have lifted out the function to a separate file.

In the **scss** folder and in the **maps** folder there is a file named **_bcss-custom-colors-scss**. Here you can create and name you own colors.

```scss linenums="1" title="scss/maps/_bcss-custom-colors.scss"
// Custom colors
$crimson: #9a0044;
$magenta: #d63384;
$gold: #fd7e14;
$turquoise: #20c997;

// Custom colors map (add to css variables)
$beets-colors: (
	"crimson": $crimson,
	"magenta": $magenta,
	"gold": $gold,
	"turquoise": $turquoise,
);

// Merge with colors map
$colors: map-merge($colors, $beets-colors);

// Custom theme colors map (add to Bootstrap theme)
$beets-theme-colors: (
	"beets": $crimson,
	"bubblegum": $magenta,
	"autumn": $gold,
	"spring": $turquoise,
);

// Merge with theme colors map
$theme-colors: map-merge($theme-colors, $beets-theme-colors);
```