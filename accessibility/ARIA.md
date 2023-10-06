#INTRODUCTION TO ARIA
Web Accessibility Initiative — Accessible
Rich Internet Applications (WAI-ARIA)
Created by W 3C
Part ofHTML
Allows you to make your website
comprehensive to screenreaders, even if it
is an interactive web application

USE OF ARIA
ARIA is not designed to replace semantic
HTML - this should always be your first
port of call
However, there are times you want to use
ARIA instead of vanilla HTML
For example, disable buttons with ARIA
rather than the disabled attribute

ROLES
The role attribute can be used to mimic a HTML element
Again, we want to use semantic HTML first, and this as a last resort
role="navigation"
role="checkbox"
role="text" (not part of specification)
Avoid redundancy <nav role="navigation">
![[role.png.png]]

LABELS
Labels can be added to anything and override other label attributes
<button class="menu-button" aria-label="Menu"></button>

HIDDEN
Remove anything from the DOM that is purely presentational
Do not hide anything important
</article>
<img src="clowns.jpg" alt="Clowns demand higher pay" aria-hidden="true">
<h2><a href="/news/clowns">Clowns demand higher pay</a></h2>
</article>


FORMS
Labelled by establishes a link between an element and a label
This is perfect for radios or checkboxes, for example
<span aria-checked="true">
When "greying out" buttons you can use ARIA
<button aria-disabled="true">Submit</button>
Add labels and additional notes (read after the form field) with ARIA
We'll see an example on the next slide

![[form.png.png]]


LIVE REGIONS
For parts of the page with dynamic updates, you can mark them as live regions
This tells screen readers to read out any changes
<div aria-live="polite">...</div>
<div aria-live="assertive">...</div>
For alerts and errors, there is already a live region role you can use
<div role="alert">Error message.</div>