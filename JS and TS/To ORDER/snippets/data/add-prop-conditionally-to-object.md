1. ex
```js
// if roleIsOk is true role prop is added to the object
const result = await create(dashboardUsers, {
    email,
    password: hashedPwd,
    ...(roleIsOk && { role }),
  });
```
2. ex
```js
{
	a: 'ciao', 
	...(false && {role: 'beppe'})
}
// {a: 'ciao'}

{
	a: 'ciao', 
	...(true && {role: 'beppe'})
}
// {a: 'ciao', role: 'beppe'}
```
3. ex array
```js
	const trueCondition = true;
	const falseCondition = false;
	
	const arr = [
	  ...(trueCondition ? ["dog"] : []),
	  ...(falseCondition ? ["cat"] : [])
	];
	
	// ['dog']
```