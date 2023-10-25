```css
& .caption-list {
	display: grid;
	grid-template-columns: repeat(2, 1fr);
	grid-gap: var(--gg-akainu);
	min-width: 0;
	min-height: 0;
	grid-template-rows: repeat(3, minmax(0, 1fr));
	height: 100%;
}
```