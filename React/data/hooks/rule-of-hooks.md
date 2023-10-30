
1. Only `call` React Hooks in React Functions
		React Component Functions
		Custom Hooks
2. Only call React Hooks at the Top Level
		Don t call them in nested function
		Don t call them in any block statement
+ extra, unofficial Rule for useEffect(): ALWAYS add everything you refer to inside of useEffect() as a dependency!