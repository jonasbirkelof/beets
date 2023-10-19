<style>
.md-typeset .demo ol.breadcrumb:not([hidden]) {
	display: flex;
	font-size: 16px;
}
[dir="ltr"] .md-typeset .demo ol,
[dir="ltr"] .md-typeset .demo ol li {
	margin-left: 0;
}
[dir="ltr"] .md-typeset .demo ol li {
	margin-bottom: 0;
}
</style>

# Breadcrumb

The only thing that is different betewwn Beets and Bootstrap is the formatting of the links. The Bootstrap links are underlined where the Beets links are only underlined while hovering.

<div class="demo">
	<nav aria-label="breadcrumb">
		<ol class="breadcrumb">
			<li class="breadcrumb-item"><a href="#">Home</a></li>
			<li class="breadcrumb-item"><a href="#">Library</a></li>
			<li class="breadcrumb-item active" aria-current="page">Data</li>
		</ol>
	</nav>
</div>

````html
<nav aria-label="breadcrumb">
	<ol class="breadcrumb">
		<li class="breadcrumb-item"><a href="#">Home</a></li>
		<li class="breadcrumb-item"><a href="#">Library</a></li>
		<li class="breadcrumb-item active" aria-current="page">Data</li>
	</ol>
</nav>
````