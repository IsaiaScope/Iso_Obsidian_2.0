```ts
export const QP = ({
  params,
  operation = 'NEW-URL',
}: {
  params: { name: string; value: string | number }[];
  operation: 'NEW-URL' | 'ADD';
}) => {
    const queryParamList = params.map(({ name, value }) => {
	 if (!value) {
      return '';
   }
    return `&${name}=${value}`;
  });

	const finalString = queryParamList.filter(v => v).join('');

	if (operation === 'ADD') {
    return finalString;
  }
  
  return finalString.replace('&', '?');
};
```