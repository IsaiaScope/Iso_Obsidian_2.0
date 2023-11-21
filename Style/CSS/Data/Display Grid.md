# Display Grid

## Not Overflow From Parent Height

- Using `grid-template-rows: repeat(3, minmax(0, 1fr));` to define rows
  - `minmax(0, 1fr)` makes the 3 rows of the grid to fill 100% height of parent element
  - not allow `.caption-list` to overflow from the parent height and occupy all available space in height
  - `grid-auto-rows` permit to repeat a specific pattern and avoid to write a specific number of rows
- Sometimes are needed also
  - min-width: 0;
  - min-height: 0;

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

---
