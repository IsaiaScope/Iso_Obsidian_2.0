---
tags:
  - React_Native
---

# Map

## Map.js

`MapView` a library that provides a Map component that uses Google Maps on Android and Apple Maps or Google Maps on iOS

```jsx
import { useCallback, useLayoutEffect, useState } from "react";
import { Alert, StyleSheet } from "react-native";
import MapView, { Marker } from "react-native-maps";

import IconButton from "../components/UI/IconButton";

function Map({ navigation, route }) {
	const initialLocation = route.params && {
		lat: route.params.initialLat,
		lng: route.params.initialLng,
	};

	const [selectedLocation, setSelectedLocation] = useState(initialLocation);

	const region = {
		latitude: initialLocation ? initialLocation.lat : 37.78,
		longitude: initialLocation ? initialLocation.lng : -122.43,
		latitudeDelta: 0.0922,
		longitudeDelta: 0.0421,
	};

	function selectLocationHandler(event) {
		if (initialLocation) {
			return;
		}
		const lat = event.nativeEvent.coordinate.latitude;
		const lng = event.nativeEvent.coordinate.longitude;

		setSelectedLocation({ lat: lat, lng: lng });
	}

	const savePickedLocationHandler = useCallback(() => {
		if (!selectedLocation) {
			Alert.alert(
				"No location picked!",
				"You have to pick a location (by tapping on the map) first!"
			);
			return;
		}

		navigation.navigate("AddPlace", {
			pickedLat: selectedLocation.lat,
			pickedLng: selectedLocation.lng,
		});
	}, [navigation, selectedLocation]);

	useLayoutEffect(() => {
		if (initialLocation) {
			return;
		}
		navigation.setOptions({
			headerRight: ({ tintColor }) => (
				<IconButton
					icon="save"
					size={24}
					color={tintColor}
					onPress={savePickedLocationHandler}
				/>
			),
		});
	}, [navigation, savePickedLocationHandler, initialLocation]);

	return (
		<MapView
			style={styles.map}
			initialRegion={region}
			onPress={selectLocationHandler}
		>
			{selectedLocation && (
				<Marker
					title="Picked Location"
					coordinate={{
						latitude: selectedLocation.lat,
						longitude: selectedLocation.lng,
					}}
				/>
			)}
		</MapView>
	);
}

export default Map;

const styles = StyleSheet.create({
	map: {
		flex: 1,
	},
});
```

---

## PlaceDetails.js

> Navigation logic for navigating to Map Component

```jsx
import { useEffect, useState } from "react";
import { ScrollView, Image, View, Text, StyleSheet } from "react-native";

import OutlinedButton from "../components/UI/OutlinedButton";
import { Colors } from "../constants/colors";
import { fetchPlaceDetails } from "../util/database";

function PlaceDetails({ route, navigation }) {
	const [fetchedPlace, setFetchedPlace] = useState();

	function showOnMapHandler() {
		navigation.navigate("Map", {
			initialLat: fetchedPlace.location.lat,
			initialLng: fetchedPlace.location.lng,
		});
	}

	const selectedPlaceId = route.params.placeId;

	useEffect(() => {
		async function loadPlaceData() {
			const place = await fetchPlaceDetails(selectedPlaceId);
			setFetchedPlace(place);
			navigation.setOptions({
				title: place.title,
			});
		}

		loadPlaceData();
	}, [selectedPlaceId]);

	if (!fetchedPlace) {
		return (
			<View style={styles.fallback}>
				<Text>Loading place data...</Text>
			</View>
		);
	}

	return (
		<ScrollView>
			<Image style={styles.image} source={{ uri: fetchedPlace.imageUri }} />
			<View style={styles.locationContainer}>
				<View style={styles.addressContainer}>
					<Text style={styles.address}>{fetchedPlace.address}</Text>
				</View>
				<OutlinedButton icon="map" onPress={showOnMapHandler}>
					View on Map
				</OutlinedButton>
			</View>
		</ScrollView>
	);
}

export default PlaceDetails;

const styles = StyleSheet.create({
	fallback: {
		flex: 1,
		justifyContent: "center",
		alignItems: "center",
	},
	image: {
		height: "35%",
		minHeight: 300,
		width: "100%",
	},
	locationContainer: {
		justifyContent: "center",
		alignItems: "center",
	},
	addressContainer: {
		padding: 20,
	},
	address: {
		color: Colors.primary500,
		textAlign: "center",
		fontWeight: "bold",
		fontSize: 16,
	},
});
```

---

## App.js

Where the screen in implemented

> <Stack.Screen name="Map" component={Map} />

```jsx
import { useEffect, useState } from "react";
import { StatusBar } from "expo-status-bar";
import { NavigationContainer } from "@react-navigation/native";
import { createNativeStackNavigator } from "@react-navigation/native-stack";
import AppLoading from "expo-app-loading";

import AllPlaces from "./screens/AllPlaces";
import AddPlace from "./screens/AddPlace";
import IconButton from "./components/UI/IconButton";
import { Colors } from "./constants/colors";
import Map from "./screens/Map";
import { init } from "./util/database";
import PlaceDetails from "./screens/PlaceDetails";

const Stack = createNativeStackNavigator();

export default function App() {
	const [dbInitialized, setDbInitialized] = useState(false);

	useEffect(() => {
		init()
			.then(() => {
				setDbInitialized(true);
			})
			.catch((err) => {
				console.log(err);
			});
	}, []);

	if (!dbInitialized) {
		return <AppLoading />;
	}

	return (
		<>
			<StatusBar style="dark" />
			<NavigationContainer>
				<Stack.Navigator
					screenOptions={{
						headerStyle: { backgroundColor: Colors.primary500 },
						headerTintColor: Colors.gray700,
						contentStyle: { backgroundColor: Colors.gray700 },
					}}
				>
					<Stack.Screen
						name="AllPlaces"
						component={AllPlaces}
						options={({ navigation }) => ({
							title: "Your Favorite Places",
							headerRight: ({ tintColor }) => (
								<IconButton
									icon="add"
									size={24}
									color={tintColor}
									onPress={() => navigation.navigate("AddPlace")}
								/>
							),
						})}
					/>
					<Stack.Screen
						name="AddPlace"
						component={AddPlace}
						options={{
							title: "Add a new Place",
						}}
					/>
					<Stack.Screen name="Map" component={Map} />
					<Stack.Screen
						name="PlaceDetails"
						component={PlaceDetails}
						options={{
							title: "Loading Place...",
						}}
					/>
				</Stack.Navigator>
			</NavigationContainer>
		</>
	);
}
```

---
