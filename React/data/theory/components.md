#### General theory overview
1. `NOTE` *components tags are not render in html page, like in Angular happens. Components tags are remove at run time*
2. If component tag doesn't have any content inside u could omit the second one e use self closing tag
```jsx
<Component title={someTitle}><Component>
// become
<Component title={someTitle} />
```
---
3. The conception of '*composition*' 
is possible to create a wrapper component like card as following and wrap inside other component to share logic and style; *props.children* is a special props field passed by default and always present, that is the content wrapped inside card tags
```jsx
function Card(props) {
   const classes = 'card ' + props.className;
   return <div className={classes}>{props.children}</div>;
}
export default Card;
``` 
---
4. ***arrow components*** is just personal preference, nothing change between two approach
```jsx
const Card => (props) {
  const classes = 'card + props.className;
   return <div className={classes)>(props.children)</div>;
}
export default Card;
``` 
---
