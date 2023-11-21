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

### Why you shouldn't set font-sizes using em



---

# Rem (Root Em)

> Font size referring to the root element (html tag).

- [px-to-rem-converter](https://nekocalc.com/px-to-rem-converter)
  - _16px = 1rem_ usually the browser default

---
