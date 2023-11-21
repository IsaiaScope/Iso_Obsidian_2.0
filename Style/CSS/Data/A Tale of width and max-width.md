# A Tale of width and max-width

An interesting look at the two properties, and how they might not always act the way you think they will.

[tale-width-max-width](https://css-tricks.com/tale-width-max-width/)

## Example

You might want to limit the width of a modal, right? Kinda gives it that “modal” look on larger screens. Let’s say 600px sounds right. But, you want to make sure it doesn’t bust outside of its parent element. For example, avoid causing horizontal scrolling on a mobile screen.

```css
.modal {
	width: 100%;
	max-width: 600px;
}
```

---
