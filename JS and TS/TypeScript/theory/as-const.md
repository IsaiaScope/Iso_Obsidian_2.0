1. N.B. object.freeze() works just on top level doesn't block change a prop deeply than first level
2. as const makes object deeply immutable and readonly
 
```ts
const routes = {
	home: '/',
	admin: '/admin',
	users: '/users',
} as const

type RouteKeys = keyof typeof routes;

type Route = (typeof routes)[RouteKeys];

// type Route = (typeof routes)[keyof typeof routes];
```

---