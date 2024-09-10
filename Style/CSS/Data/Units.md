# Units

[The flowchar by Kevin Powell](https://whatunit.com/)

## vh, vw, vmin, vmax

### Viewport height (vh) & viewport width (vw)

W3C defines viewport as “size of the initial containing block”. In other words, viewport is the area contained within the browser window or any other viewing area on a screen.

The `vw` and `vh` units stand for the percentage of the width and height of the actual viewport. They can take a value between 0 and 100 according to the following rules:

> 100vw = 100% of viewport width
> 1vw = 1% of viewport width

> 100vh = 100% of viewport height
> 1vh = 1% of viewport height

#### Example

```css
.hero {
  background: linear-gradient(to left, #83a4d4 , #b6fbff);
  padding: 10vh 0;
  height: 100vh;
}
```

---

#### Differences to percentage units

So, how are viewport units different from percentage units?
Percentage units inherit the size of their parent element while viewport units don’t.

Viewport units are always calculated as the percentage of the viewport size.
As a result, an element defined by viewport units can exceed the size of its parent.

---

### Viewport min (vmin) & viewport max (vmax)

The `vmin` and `vmax` units allow you to access the size of the smaller or the larger side of the viewport, according to the following rules:

> 100vmin = 100vw or 100vh, whichever is smaller
> 1vmin = 1vw or 1vh, whichever is smaller

> 100vmax = 100vw or 100vh, whichever is larger
> 1vmax = 1vw or 1vh, whichever is larger

So, in case of a *portrait orientation*, `100vmin` is equal to `100vw`, as the viewport is *smaller horizontally than vertically*. For the same reason, `100vmax` will be equal to `100vh`.

Similarly, in case of a *landscape orientation*, `100vmin` is equal to `100vh`, as the viewport is *smaller vertically than horizontally*. And, of course, `100vmax` will be equal to `100vw` here.

they can be excellently used as a substitute for orientation `@media` queries. For instance, `vmin` and `vmax` can come in handy when you have elements that may look strange at different aspect ratios.

---

## min(), max(), clamp()

For `min()` and `max()`, you provide an argument list of values, and the browser determines which one is either the smallest or largest, respectively. For example, in the case of: `min(1rem, 50%, 10vw)`, the browser calculates which of these relative units is the smallest, and uses that value as the actual value.

To use `clamp()` enter three values: a minimum value, ideal value (from which to calculate), and maximum value.

To recap:

- `min(<value-list>)`: selects the smallest (most negative) value from a list of comma-separated expressions
- `max(<value-list>)`: selects the largest (most positive) value from a list of comma-separated expressions
- `clamp(<min>, <ideal>, <max>)`: clamps a value between an upper and lower bound, based on a set ideal value

> When using a calculation inside of a `min()`, `max()`, or `clamp()` function, you can remove the call to `calc()`. For example, writing `font-size: max(calc(0.5vw - 1em), 2rem)` would be the same as `font-size: max(0.5vw - 1em, 2rem)`.

They are some exciting functions that we can use as for the values of elements, so we could set something like `width: min(95%, 1200px);`, which would be the same as _setting both a width and a max-width_.

And while setting a width and max-width is something we could already do, we can also use this for things like margin and padding (and font sizes!), where something like that wasn't even possible before.

_Clamp() is even more exciting in how we could use it for responsive typography_

### Example (font-size with clamp() to set responsiveness)

adding rems keeps the responsiveness when user zoom in or out

```css
--fs-xl: clamp(3.5rem, 12vw + 1rem, 12rem);
```

---
