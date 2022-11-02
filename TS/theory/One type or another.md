- u can use never keyword to block a key to be issued on const creation
- ***icon?: never;*** in interface SvgImg u cannot define icon key
- ***img?: never;*** in interface SvgIcon u cannot define img key
```typescript

interface SvgDataBaseline {
  type: 'icon' | 'img';
}

interface SvgIcon extends SvgDataBaseline {
  icon: {
    iconName: string;
    svgClasses?: string;
    path?: string;
  };
  img?: never;
}

interface SvgImg extends SvgDataBaseline {
  icon?: never;
  img: {
    imgName: string;
    svgClasses?: string;
    path?: string;
  };
}

export type SvgData = SvgIcon | SvgImg;

export const svgIconDefault: SvgIcon = {
  type: 'icon',
  icon: {
    iconName: '',
    svgClasses: 'svg-icon-default',
    path: '../../../assets/svgs/icons/',
  },
};

export const svgImgDefault: SvgImg = {
  type: 'img',
  img: {
    imgName: '',
    svgClasses: 'svg-img-default',
    path: '../../../assets/svgs/imgs/',
  },
};

```
---