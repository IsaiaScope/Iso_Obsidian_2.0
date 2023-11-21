# Selecting by Attribute

## Theory

[w3schools - css_attribute_selectors](https://www.w3schools.com/css/css_attribute_selectors.asp)

## Example with Style Attribute

- The `[attribute*="value"]` selector is used to select elements whose attribute value contains a specified value.

```scss
div:has(.icon-chevron-left[style*='visibility: visible']) {
	align-items: center;
	.icon-chevron-left {
		margin-right: 1em;
	}
}
```

---
