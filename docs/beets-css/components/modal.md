<style>
.modal-content {
	border-radius: 6px;
}
.modal-header,
.modal-footer {
	padding: 12px 24px;
}
.modal-header .modal-title {
	margin: 0;
	letter-spacing: 0.04rem;
	font-weight: 500;
	font-size: 20px;
	text-transform: initial;
	color: var(--bs-dark) !important;
}
.modal-body {
	padding: 24px;
}
.modal-body p {
	margin-bottom: 16px;
}
</style>

# Modal

The modals have just been styled slightly to look like the [cards](../card) with a little wider x-axis padding on the body, header and footer as well as a shadow.

The last `<p>` tag in the body will have `margin-bottom: 0;` automaticlly. You can add `.mb-3` to the last `<p>` tag to bypass this.

<div class="demo">
	<div class="modal text-dark px-7 py-5" style="display: block; position: initial;">
		<div class="modal-dialog">
			<div class="modal-content">
				<div class="modal-header">
					<h5 class="modal-title">Modal title</h5>
					<button class="btn-close" data-bs-dismiss="modal"></button>
				</div>
				<div class="modal-body">
					<p>Modal body text goes here.</p>
					<p>Modal body text goes here.</p>
				</div>
				<div class="modal-footer justify-content-between">
					<button class="btn btn-primary">Save changes</button>
					<button class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
				</div>
			</div>
		</div>
	</div>
</div>