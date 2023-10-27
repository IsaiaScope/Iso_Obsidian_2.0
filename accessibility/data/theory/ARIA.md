**_INTRODUCTION TO ARIA_** Web Accessibility Initiative Accessible

- Rich Internet Applications (WAI-ARIA)
- Created by W3C
- Part of HTML
- Allows you to make your website comprehensive to screenreaders, even if it is an interactive web application

---

_USE OF ARIA_

- ARIA is not designed to replace semantic => HTML should always be your first port of call
- However, there are times you want to use ARIA instead of vanilla HTML
  - For example, disable buttons with ARIA rather than the disabled attribute

---

- _aria-expanded_ (boolean) => to tell to screen reader if is open or close

```html
<button id="hamburger" aria-expanded="false"></button>
```

---

- _aria-label_
  - svg inside anchor insert aria-label in svg to describe the svg icon
  - more effective approach [[icon | aria-label inside anchor tag and insert aria-hidden in the svg]]

```html
<a href="https://www.instagram.com">
	<svg
		version="1.1"
		aria-label="Follow company on Instagram"
		xmlns="http://www.w3.org/2000/svg"
		xmlns:xlink="http://www.w.org/1999/Xlink"
		width="32"
		height="32"
		viewBox="0 0 16 16"
	>
		*
	</svg>
</a>
```

- or labels can be added to anything and override other label attributes

```html
<button class="menu-button" aria-label="Menu"></button>
```

---

- _aria-labelledby_

```html
<img src="xxx" alt="" aria-labelledby="caption" width="200" />
<p id="caption">Captions</p>
```

---

- _aria-describedby_
  - The global `aria-describedby` attribute identifies the element (or elements) that describes the element on which the attribute is set.
  - [[link | Example in Link Note File]]

```html
<button aria-describedby="trash-desc">Move to trash</button>
…
<p id="trash-desc">
	Items in the trash will be permanently removed after 30 days.
</p>
```

---

- _aria-live_
- For parts of the page with dynamic updates, you can mark them as live regions; this tells screen readers to read out any changes

```html
<div aria-live="polite">...</div>
<div aria-live="assertive">...</div>
For alerts and errors, there is already a live region role you can use
<div role="alert">Error message.</div>
```

---

- _aria-hidden_
- Remove anything from the DOM that is purely presentational
- Do not hide anything important

```html
<article>
	<img src="clowns.jpg" alt="Clowns demand higher pay" aria-hidden="true" />
	<h2><a href="/news/clowns">Clowns demand higher pay</a></h2>
</article>
```

---

- _aria-invalid_ useful for _HANDLING ERRORS_
- This is the most difficult part of making forms accessible
- It must be clear to the user that the form is invalid and what needs to be changed
- Ideally, take the user to the source of the error
- You can use ariadescribedby to identify the error for that field

```html
<label for="email"> Email address</label>
<input
	type="email"
	name="name"
	aria-invalid="true"
	aria-describedby="email-error-details"
/>

<p id="email-error-details">This email address is already registered.</p>
```

---
