
1. the important tag is `body` where the following style must be applied to avoid extra annoing margin
```css
*,
*::before,
*::after { 
    margin: 0; 
    padding: 0; 
    box-sizing: border-box; 
}
```