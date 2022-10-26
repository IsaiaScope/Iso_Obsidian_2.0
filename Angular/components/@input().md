#### Set
-   when a new input is issued to component, set function is triggered, is like ngOnChanges()
- ==N.B.== set doesn't trigger if input is not provided
- with this approach u can set not mandatory prop and when input is set that props are taken from default, like in the following example path ain't mandatory but if u want that set in every input even if isn't provided u can do:
```typescript
export interface SvgIcon {
  iconName: string;
  svgClasses?: string;
  path?: string;
}

export const svgIconDefault: SvgIcon = {
  path: '../../../assets/svgs/icons/',
  svgClasses: 'svg-default',
  iconName: '',
};

// input => { iconName: 'password' }

export class Child {
_setUp = svgIconDefault

  @Input() set setUp(v: SvgIcon) {
    this._setUp = { ...svgIconDefault, ...v};
  }
```
---