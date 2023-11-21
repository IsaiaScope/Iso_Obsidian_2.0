# Display Flex

## align-self

`align-self` consent to positioning like it wants inside the space to it delivered. by flex box. Flex box create columns in a row that have all the same height so `align-self` is perfect to set the element inside the column in a specific position and avoid stretching as happen with images

[[align-self.png]]

```css
.her__img {
	max-width: 100%;
	width: 32%;
	align-self: flex-start;
}
```

---

## Not Overflow From Parent Height

- set height 100% and and set `min-height: 0;`
  - to not allow the child to overflow from the parent height and occupy all available space in height

```css
height: 100%;
min-height: 0;
```

---
