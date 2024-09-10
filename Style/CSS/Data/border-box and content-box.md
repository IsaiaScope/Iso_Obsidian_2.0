# border-box and content-box

> `border-box` and `content-box` are two values of the `box-sizing` property. Unlike the `content-box` , the  `border-box` value indicates that the dimension of an element will also include the border and padding.

The `box-sizing` property can be used to adjust this behavior:

- `content-box` gives you the default CSS box-sizing behavior. If you set an element's width to 100 pixels, then the element's content box will be 100 pixels wide, and the width of any border or padding will be added to the final rendered width, making the element wider than 100px.
- `border-box` tells the browser to account for any border and padding in the values you specify for an element's width and height. If you set an element's width to 100 pixels, that 100 pixels will include any border or padding you added, and the content box will shrink to absorb that extra width. This typically makes it much easier to size elements.

## Example

Let's assume that we have a div element whose size is `200px x 100px`, the border and padding are `5px` and `10px` respectively.

```css
.div {
		box-sizing: content-box;
    width: 200px;
    border: 5px;
    padding: 10px;
}
```

In _content box_ the content's width is `200px`, but the total element (div) width is `230px`

---

```css
.div {
		box-sizing: border-box;
    width: 200px;
    border: 5px;
    padding: 10px;
}
```

In the _border box model_, the content's dimension has to subtract the border and padding from the element's dimension. Specifically, the content's width is  `200 - 5 * 2 - 10 * 2 = 170px`.

---

# Remove Extra Margin Mobile&Tablet

> the important tag where the following style must be applied is `body`, to avoid extra annoying margin

```css
*,
*::before,
*::after {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
```

---
