---
tags:
  - React_Native
---

# Theory

https://github.com/academind/react-native-practical-guide-code/tree/06-navigation

https://reactnative.dev/
https://github.com/academind/react-native-practical-guide-code/tree/main
expo lib semplify dev

> npx create-expo-app my-app

cannot insert text into a view; need text component

flex box - def-vertical

Question 7: What's the default styling/ layout behavior of a ‹ View› component?
It uses Flexbox to organize its child components.
flex: 1 take all space available vertically

console log are in terminal console

? in termina shows shortcut

npm i -g reac-devtools

elevation => box shadow for andiod
shadow prop => box shadow for ios

scrollview

platform API
// borderWidth: Platform.OS === 'android' ? 2 : 0,
// borderWidth: Platform.select({ ios: 0, android: 2 }),

navigatorn makes header and safe area out the box

Setting the Default Screen

When setting up a Navigator (like `<Stack.Navigator>`) and registering its screens (via `<Stack.Screen>`), you can decide **which screen will be shown as a default when the app starts**.

Out of the box, the **top-most screen** (i.e. the **first child** inside of `<Stack.Navigator>`) is used as the initial screen.

I.e., in the following example, the AllProducts screen would be shown as an initial screen when the app starts:

1. <Stack.Navigator>
2. <Stack.Screen name="AllProducts" component={AllProducts} /> // initial screen
3. <Stack.Screen name="ProductDetails" component={ProductDetails} />
4. </Stack.Navigator>

You can therefore change the initial screen by changing the `<Stack.Screen>` order. Alternatively, there also is an `initialRouteName` prop that can be set on the navigator component (i.e., on `<Stack.Navigator>` in this case):

1. <Stack.Navigator initialRouteName="ProductDetails">
2. <Stack.Screen name="AllProducts" component={AllProducts} />
3. <Stack.Screen name="ProductDetails" component={ProductDetails} /> // initial screen
4. </Stack.Navigator>

get navigation prop in component loaded as screen

useRoute for params


img picker ios needs permission to work, permission must be coded instead android has the allow modal by default

when change screen components arent recreated just put on stuck so the solution isFocused hook, so if you go bach into the stuck the components are not recreated and things insiede the logic as useeffects aren't triggered