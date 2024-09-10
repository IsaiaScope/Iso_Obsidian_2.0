---
tags:
  - React_Native
---

# Adaptive User Interface

## useWindowDimensions

- `useWindowDimensions` is the preferred API for React components. Unlike `Dimensions`, it updates as the window's dimensions update. This works nicely with the React paradigm.

```jsx
import { useWindowDimensions } from "react-native";

const { width, height } = useWindowDimensions();
```

---
