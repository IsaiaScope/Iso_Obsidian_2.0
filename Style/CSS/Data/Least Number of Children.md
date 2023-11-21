# Least Number of Children

## Example

In a list (of HTML Elements) of `.caption`; add style if the list has 2 children

```css
.caption:first-child:nth-last-child(2),
.caption:first-child:nth-last-child(2) ~ .caption {
	max-height: var(--mh-kuro);
}
```

---
