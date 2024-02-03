---
tags:
  - React_Native
---

# Navigation

No URL like browser, logic is based on screens

## App.js

- `createNativeStackNavigator` basic navigation better that just stack one but more buggy so in casa switch back
- `createDrawerNavigator` side menù

```jsx
import { StatusBar } from "expo-status-bar";
import { StyleSheet, Button } from "react-native";
import { NavigationContainer } from "@react-navigation/native";
import { createNativeStackNavigator } from "@react-navigation/native-stack";
import { createDrawerNavigator } from "@react-navigation/drawer";
import { Ionicons } from "@expo/vector-icons";

import CategoriesScreen from "./screens/CategoriesScreen";
import MealsOverviewScreen from "./screens/MealsOverviewScreen";
import MealDetailScreen from "./screens/MealDetailScreen";
import FavoritesScreen from "./screens/FavoritesScreen";

const Stack = createNativeStackNavigator();
const Drawer = createDrawerNavigator();

function DrawerNavigator() {
	return (
		<Drawer.Navigator
			screenOptions={{
				headerStyle: { backgroundColor: "#351401" },
				headerTintColor: "white",
				sceneContainerStyle: { backgroundColor: "#3f2f25" },
				drawerContentStyle: { backgroundColor: "#351401" },
				drawerInactiveTintColor: "white",
				drawerActiveTintColor: "#351401",
				drawerActiveBackgroundColor: "#e4baa1",
			}}
		>
			<Drawer.Screen
				name="Categories"
				component={CategoriesScreen}
				options={{
					title: "All Categories",
					drawerIcon: ({ color, size }) => (
						<Ionicons name="list" color={color} size={size} />
					),
				}}
			/>
			<Drawer.Screen
				name="Favorites"
				component={FavoritesScreen}
				options={{
					drawerIcon: ({ color, size }) => (
						<Ionicons name="star" color={color} size={size} />
					),
				}}
			/>
		</Drawer.Navigator>
	);
}

export default function App() {
	return (
		<>
			<StatusBar style="light" />
			<NavigationContainer>
				<Stack.Navigator
					screenOptions={{
						headerStyle: { backgroundColor: "#351401" },
						headerTintColor: "white",
						contentStyle: { backgroundColor: "#3f2f25" },
					}}
				>
					<Stack.Screen
						name="Drawer"
						component={DrawerNavigator}
						options={{
							headerShown: false,
						}}
					/>
					<Stack.Screen name="MealsOverview" component={MealsOverviewScreen} />
					<Stack.Screen
						name="MealDetail"
						component={MealDetailScreen}
						options={{
							title: "About the Meal",
						}}
					/>
				</Stack.Navigator>
			</NavigationContainer>
		</>
	);
}

const styles = StyleSheet.create({
	container: {},
});
```

---

## MealItem.js

- `useNavigation` hook for get back navigation obj

```jsx
import {
	View,
	Pressable,
	Text,
	Image,
	StyleSheet,
	Platform,
} from "react-native";
import { useNavigation } from "@react-navigation/native";

import MealDetails from "./MealDetails";

function MealItem({
	id,
	title,
	imageUrl,
	duration,
	complexity,
	affordability,
}) {
	const navigation = useNavigation();

	function selectMealItemHandler() {
		navigation.navigate("MealDetail", {
			mealId: id,
		});
	}

	return (
		<View style={styles.mealItem}>
			<Pressable
				android_ripple={{ color: "#ccc" }}
				style={({ pressed }) => (pressed ? styles.buttonPressed : null)}
				onPress={selectMealItemHandler}
			>
				<View style={styles.innerContainer}>
					<View>
						<Image source={{ uri: imageUrl }} style={styles.image} />
						<Text style={styles.title}>{title}</Text>
					</View>
					<MealDetails
						duration={duration}
						affordability={affordability}
						complexity={complexity}
					/>
				</View>
			</Pressable>
		</View>
	);
}

export default MealItem;

const styles = StyleSheet.create({
	mealItem: {
		margin: 16,
		borderRadius: 8,
		overflow: Platform.OS === "android" ? "hidden" : "visible",
		backgroundColor: "white",
		elevation: 4,
		shadowColor: "black",
		shadowOpacity: 0.25,
		shadowOffset: { width: 0, height: 2 },
		shadowRadius: 8,
	},
	buttonPressed: {
		opacity: 0.5,
	},
	innerContainer: {
		borderRadius: 8,
		overflow: "hidden",
	},
	image: {
		width: "100%",
		height: 200,
	},
	title: {
		fontWeight: "bold",
		textAlign: "center",
		fontSize: 18,
		margin: 8,
	},
});
```

---

## CategoriesScreen.js

- `navigation` is an automatic prop if the component is a first child of a screen root, directly under the root screen component

```jsx
import { FlatList } from "react-native";
import CategoryGridTile from "../components/CategoryGridTile";

import { CATEGORIES } from "../data/dummy-data";

function CategoriesScreen({ navigation }) {
	function renderCategoryItem(itemData) {
		function pressHandler() {
			navigation.navigate("MealsOverview", {
				categoryId: itemData.item.id,
			});
		}

		return (
			<CategoryGridTile
				title={itemData.item.title}
				color={itemData.item.color}
				onPress={pressHandler}
			/>
		);
	}

	return (
		<FlatList
			data={CATEGORIES}
			keyExtractor={(item) => item.id}
			renderItem={renderCategoryItem}
			numColumns={2}
		/>
	);
}

export default CategoriesScreen;
```

---

## MealsOverviewScreen.js

- `useLayoutEffect` is a version of useEffect that fires before the browser repaints the screen.

```jsx
import { useLayoutEffect } from "react";
import { View, FlatList, StyleSheet } from "react-native";

import MealItem from "../components/MealItem";
import { MEALS, CATEGORIES } from "../data/dummy-data";

function MealsOverviewScreen({ route, navigation }) {
	const catId = route.params.categoryId;

	const displayedMeals = MEALS.filter((mealItem) => {
		return mealItem.categoryIds.indexOf(catId) >= 0;
	});

	useLayoutEffect(() => {
		const categoryTitle = CATEGORIES.find((category) => category.id === catId)
			.title;

		navigation.setOptions({
			title: categoryTitle,
		});
	}, [catId, navigation]);

	function renderMealItem(itemData) {
		const item = itemData.item;

		const mealItemProps = {
			id: item.id,
			title: item.title,
			imageUrl: item.imageUrl,
			affordability: item.affordability,
			complexity: item.complexity,
			duration: item.duration,
		};
		return <MealItem {...mealItemProps} />;
	}

	return (
		<View style={styles.container}>
			<FlatList
				data={displayedMeals}
				keyExtractor={(item) => item.id}
				renderItem={renderMealItem}
			/>
		</View>
	);
}

export default MealsOverviewScreen;

const styles = StyleSheet.create({
	container: {
		flex: 1,
		padding: 16,
	},
});
```

---
