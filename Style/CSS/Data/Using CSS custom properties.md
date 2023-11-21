# Using CSS custom properties

## Resource



## Example

- [See on Codepen](https://codepen.io/kevinpowell/pen/RwEqEyV)

```css
:root {
  --clr-white: hsl(220, 37%, 100%);
  --clr-text: hsl(244, 44%, 14%);

  --clr-yellow-400: hsl(42, 99%, 69%);
  --clr-yellow-300: hsl(47, 100%, 90%);
  --clr-purple-400: hsl(280, 96%, 45%);
  --clr-purple-300: hsl(282, 89%, 90%);
  --clr-cyan-400: hsl(198, 99%, 49%);
  --clr-cyan-300: hsl(240, 100%, 90%);
}

.plan {
  --_shadow: var(--shadow, pink);
  --_icon: var(--icon, red);
  --_button-hover: var(--button-hover, var(--clr-text));


  background: var(--clr-white);
  box-shadow: 0 0 1rem rgb(0 0 0 / 0.122), -1rem -1rem 0 0 var(--_shadow);
}

.plan__icon {
  width: 2.5rem;
  fill: var(--_icon);
}

.plan .button:hover,
.plan .button:focus {
  background-color: var(--_button-hover);
}

.plan--pram {
  --button-hover: var(--clr-yellow-400);
  --shadow: var(--clr-yellow-300);
  --icon: var(--clr-yellow-400);
}

.plan--bike {
  --shadow: var(--clr-cyan-300);
  --icon: var(--clr-cyan-400);
  --button-hover: var(--clr-cyan-400);
}

.plan--rocket {
  --shadow: var(--clr-purple-300);
  --icon: var(--clr-purple-400);
  --button-hover: var(--clr-purple-400);
}

.plan--light {
  --shadow: lightgreen;
  --icon: lime;
  --button-hover: lime;
}

```

---
