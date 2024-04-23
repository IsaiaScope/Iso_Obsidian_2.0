# icomoon

## Theory

> Font for Icons [icomoon](https://icomoon.io/app)

Steps to create/edit the Font (`RTE`)

- [ ] Import existing `icomoon.svg` file
- [ ] add any svg icon (just drag and drop work fine) and select any icon desired and click _Generate Font_ on the bottom right
- [ ] import All file in _icomoon.zip\fonts_ into the project
- [ ] edit _icons.css_ adding the same _glyph-name="name"_ as in _icomoon.svg_

## Usage

> Note: fonts is apply by [class^="icon-"], [class*=" icon-"] { ... }

```html
<span class="icon-recom"></span>
```

overwrite the style

```css
span:before {
	color: var(--black);
}
```

---
