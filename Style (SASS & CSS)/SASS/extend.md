1. extend
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