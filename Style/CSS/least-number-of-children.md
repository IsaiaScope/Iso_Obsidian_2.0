1. in a list of .caption; add style if the list has 2 child
```css
.caption:first-child:nth-last-child(2),
.caption:first-child:nth-last-child(2) ~ .caption {
	max-height: var(--mh-kuro);
}
```