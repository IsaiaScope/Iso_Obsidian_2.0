- This is perfect for radios or checkboxes, for example

```html
<div>
	<input type="checkbox" id="horns" name="horns" aria-checked="true" />
	<label for="horns">Horns</label>
</div>
```

---

_Buttons_

- When to use
  -To perform some functionality such as submitting a form or making on-page changes.
- When not to use
  - A simple link to another webpage.

_DISABLING BUTTONS_

- Avoid using the disabled attribute on buttons
- Adding the disabled attribute takes the button out of the tab index and therefore essentially disappears from the page
- If the form is not in a submit-able state, allow the user to submit anyway and tell them what they need to change
- _When "greying out" buttons you can use ARIA_; the solution is aria-disabled="true"

```html
<button aria-disabled="true">Submit</button>
```
