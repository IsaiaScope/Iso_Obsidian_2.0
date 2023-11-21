# Units

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
