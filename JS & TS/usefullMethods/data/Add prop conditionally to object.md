```js
// if roleIsOk is true role prop is added to the object
const result = await create(dashboardUsers, {
    email,
    password: hashedPwd,
    ...(roleIsOk && { role }),
  });
```