# Em

> Relative to font size of the parent, in the case of typographical properties like font-size, and font size of the element itself, in the case of other properties like width, margin, padding ecc...

## Example

Setting the _padding: 2em_ and the relative font size of the same element to 14px, means the padding applied will be 28px!

Is possible to use `em` to set font size to; if u set _font-size: 2em_ to an element; it takes current font-size _from parent_ or body and multiply that value by 2

```css
.btn {
  display: inline-block;
  background: black;
  font-weight: 700;
  letter-spacing: 5px;
  color: white;
  text-transform: uppercase;
  text-decoration: none;
	font-size: 20px;

  padding: 1em 3em;

  margin: 0 .25em;
}

.btn--small {
  font-size: .5em;
}

.btn--lrg {
  font-size: 1.5rem;
}
```

this create an adaptative situation where changing `.btn` font size margin and padding keep proportion without need adjusting.

font-size could be declared with px, rem, em but `padding: 1em 3em;`, `margin: 0 .25em;` refer to it and change their value based on it

## Why you shouldn't set font-sizes using em/px

Would recommend setting all sizes using `rem`. I sprinkle in `em` only where I want something to be proportional to the current font size (e.g., an icon next to some text that should be exactly the same height as the characters, and half a character to the side).

> Never set `font-size` in `px` units—at least, not unless you’re incredibly sure of what you’re doing, how it will behave, and whether it will still be accessible when you do.

---

# Rem (Root Em)

> Font size referring to the root element (html tag).

- [px-to-rem-converter](https://nekocalc.com/px-to-rem-converter)
  - _16px = 1rem_ usually the browser default

## An important note about media queries

It’s important to avoid `px` in `@media` queries for all the same reasons above; it will work fine when the user zooms, but a media query that uses `px` will fail users when they set a larger font size on their own.

```css
@media (min-width: 800px) {
	/* Changing font size does NOT affect this breakpoint */
}

@media (min-width: 50rem) {
	/* Changing font size DOES affect this breakpoint */
}
```

Most likely, when we’re writing CSS for larger breakpoints,_ we’re taking for granted that there’s plenty of screen real estate for elements to spread into. This may not be the case if the user has set a very large font size_, and setting our media queries in `rem` instead of `px` helps us avoid that assumption and respond to user preference.

---
