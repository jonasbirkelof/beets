<style>
/* Card styling */
.demo .card {
	border-radius: 6px;
}
.demo .card-header,
.demo .card-footer {
	padding: 12px 24px;
}
.demo .card-body {
	padding: 24px;
}
.demo .card-title {
	margin-bottom: 8px;
	margin-top: 0;
}
/* Card table */
.demo .card-table {
	margin-left: -24px;
	margin-right: -24px;
}
.demo .card-table tr th,
.demo .card-table tr td {
	padding: 8px;
}
.demo .card-table tr th:first-child,
.demo .card-table tr td:first-child {
	padding-left: 24px;
}
.demo .card-table tr th:last-child,
.demo .card-table tr td:last-child {
	padding-right: 24px;
}
.demo .card-table tr th {
	font-weight: 700;
}
</style>

# Card

The changes made to the card classes are mostly related to spacings.

The header and footer has white backgrounds as default instead of light gray. Use the Bootstrap class `.bg-light` on `.card-header` and `.card-footer` to make it look like the Bootstrap default.

## Example 

<div class="demo">
	<div class="card text-dark" style="width: 320px;">
		<div class="card-header">Header</div>
		<div class="card-body">
			<h3 class="card-title">Card title</h3>
			<h6 class="card-subtitle mb-2 text-muted">Card subtitle</h6>
			<p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
			<button class="btn btn-primary">Go somewhere</button>
		</div>
		<div class="card-footer">Footer</div>
	</div>
</div>
````html
<div class="card">
	<div class="card-header">Header</div>
	<div class="card-body">
		<h3 class="card-title">Card title</h3>
		<h6 class="card-subtitle mb-2 text-muted">Card subtitle</h6>
		<p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
		<button class="btn btn-primary">Go somewhere</button>
	</div>
	<div class="card-footer">Footer</div>
</div>
````

## Card Table

You can have a table bypass the `.card-body` gutter and span the full width of the card for a stylish look. Just wrap the `<table>` tag with a div with `.card-table` and remove the padding on the y-axis on `.card-body` and the bottom margin of the table with `.margin-bottom`.

The styling can be used to gice the table rounded corners which can't be done by using the `.rounded-*` classes.

<div class="demo">
	<div class="card">
		<div class="card-body py-0">			
			<div class="card-table">
				<table class="table mb-0">
					<thead>
						<tr>
							<th>#</th>
							<th>1</th>
							<th>2</th>
							<th>3</th>
						</tr>
					</thead>
					<tbody>
						<tr>
							<td>1</td>
							<td>A</td>
							<td>A</td>
							<td>A</td>
						</tr>
						<tr>
							<td>2</td>
							<td>B</td>
							<td>B</td>
							<td>B</td>
						</tr>
					</tbody>
				</table>
			</div>			
		</div>
	</div>
</div>
````html
<div class="card">
	<div class="card-body py-0">			
		<div class="card-table">
			<table class="table mb-0">
				...
			</table>
		</div>			
	</div>
</div>
````

<div class="demo mt-4">
	<div class="card text-dark">
		<div class="card-body pb-0">
			<h3 class="card-title">This is a headline</h3>
       		<p class="card-text">Lorem ipsum dolor sit amet consectetur adipisicing elit. Odit molestias labore consequuntur ut temporibus vero quasi recusandae culpa, esse doloremque ullam odio distinctio quibusdam laborum adipisci nam porro, quos quisquam veniam voluptatibus rerum unde.</p>
			<div class="card-table">
				<table class="table mb-0">
					<thead>
						<tr>
							<th>#</th>
							<th>1</th>
							<th>2</th>
							<th>3</th>
						</tr>
					</thead>
					<tbody>
						<tr>
							<td>1</td>
							<td>A</td>
							<td>A</td>
							<td>A</td>
						</tr>
						<tr>
							<td>2</td>
							<td>B</td>
							<td>B</td>
							<td>B</td>
						</tr>
						<tr>
							<td>3</td>
							<td>C</td>
							<td>C</td>
							<td>C</td>
						</tr>
					</tbody>
				</table>
			</div>			
		</div>
	</div>
</div>
````html
<div class="card">
	<div class="card-body pb-0">
		<h3 class="card-title">This is a headline</h3>
		<p class="card-text">...</p>
		<div class="card-table">
			<table class="table mb-0">
				...
			</table>
		</div>			
	</div>
</div>
````