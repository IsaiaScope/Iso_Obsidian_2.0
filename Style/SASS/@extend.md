# @extend

Unlike *mixins*, which copy styles into the current style rule, `@extend` updates style rules that contain the extended selector so that they contain the extending selector as well.
In other word extend generates a more light CSS, when SASS is build, because groups together same classes with `@exend` creating just one CSS big selector

## Example (Placeholder Selectors %)

Sometimes you want to write a style rule that’s *only* intended to be extended. In that case, you can use placeholder selectors, which look like class selectors that start with `%`.
Any selectors that include placeholders aren’t included in the CSS output, but selectors that extend them are.

```scss
%name {
  width: 100%;
  text-align: center;
  margin-right: 5px;
}


.alone {
    .name {
      @extend %name;
      padding-left: 26px;
    }
  }
```

---
