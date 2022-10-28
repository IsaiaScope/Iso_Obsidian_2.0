#### ActivationEnd event
- in the next example I'm retriving the guid param and the subscription trigger only if that param change in value, using .
```typescript
constructor(private router: Router)

	this.router.events.pipe(
			filter((e) => e instanceof ActivationEnd),
			map((e) => e instanceof ActivationEnd && e.snapshot.params),
			distinctUntilKeyChanged("guid"),
			map(({ guid }) => guid)
	 ).subscribe(console.log)

```
---