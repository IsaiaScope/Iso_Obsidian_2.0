- minmax(0, 1fr) makes the grid to fill 100% height
- sometimes are needed
	- min-width: 0;
	- min-height: 0;
- grid-auto-rows: permit to repeat a specific pattern and avoid to write a specific number of rows
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