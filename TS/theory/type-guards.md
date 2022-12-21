- when we have union types maybe could be useful know specific type; common approach is the following: create a method that check for a specific prop of the type if that exist that object is of that type
```typescript
type Dog = {
  name: string;
  age: number;
};

type Person = {
  firstName: string;
  lastName: string;
  age: number;
};

const trixi: Dog = {
  name: 'Trixi',
  age: 49, 
};

const robin: Person = {
  firstName: 'Robin',
  lastName: 'Wieruch',
  age: 7,
};

type Mammal = Person | Dog;
  
const isDog = (mammal: Mammal): mammal is Dog => {
  return (mammal as Dog).name !== undefined;
}; 

const celebrateBirthday = (mammal: Mammal) => {
  return {
    ...mammal,
    age: isDog(mammal) ? mammal.age + 7 : mammal.age + 1,
  };
};  

console.log(isDog(robin), typeof robin);
```
---