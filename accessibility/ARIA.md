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

_ROLES_

- The role attribute can be used to mimic a HTML element
- Again, _we want to use semantic HTML first, and this as a last resort_
- role="navigation"
- role="checkbox"
- role="text" (not part of specification)
- Avoid redundancy <nav role="navigation">
- [[role.png.png | IMG of role example]]

---

_LABELS_

- Labels can be added to anything and override other label attributes

```html
<button class="menu-button" aria-label="Menu"></button>
```

---

_HIDDEN_

- Remove anything from the DOM that is purely presentational
- Do not hide anything important

```html
<article>
	<img src="clowns.jpg" alt="Clowns demand higher pay" aria-hidden="true" />
	<h2><a href="/news/clowns">Clowns demand higher pay</a></h2>
</article>
```

---

_LIVE REGIONS_

- For parts of the page with dynamic updates, you can mark them as live regions; this tells screen readers to read out any changes

```html
<div aria-live="polite">...</div>
<div aria-live="assertive">...</div>
For alerts and errors, there is already a live region role you can use
<div role="alert">Error message.</div>
```
