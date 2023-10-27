- Add labels and additional notes (read after the form field) with ARIA
  - [[form.png.png | IMG of form example]]
- [DOC of Forms Tutorial](https://www.w3.org/WAI/tutorials/forms/)
  - witch fields are mandatory => [DOC of Form Instructions](https://www.w3.org/WAI/tutorials/forms/instructions/)
- [DOC of Creating Accessible Forms](https://webaim.org/techniques/forms/)
- [DOC of Why Placeholders in Form Fields Are Harmful](https://www.nngroup.com/articles/form-design-placeholders/)
- required attribute is read by screen reader in HTML5 or is possible to use _aria-required=true_
  - [DOC of aria-required](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-required)

---

```html
<section>
	<h2 class="mb-3 h2">Send us an email!</h2>
	<p>All fields marked with an asterisk (*) are required.</p>
	<form class="contact_form">
		<div class="form-group">
			<label>
				Your Name <span aria-hidden="true" style="color:red;">*</span>
				<input type="text" class="form-control" required />
			</label>
		</div>
		<div class="form-group">
			<label>
				Email <span aria-hidden="true" style="color:red;">*</span>
				<input type="email" class="form-control" required />
			</label>
		</div>
		<label>
			Your Message
			<span aria-hidden="true" style="color:red;">*</span>
			<textarea class="form-control" id="msg" required></textarea>
		</label>
		<div class="input-group">
			<button type="submit" class="btn btn-primary mt-3">Send Message</button>
		</div>
	</form>
</section>
```
