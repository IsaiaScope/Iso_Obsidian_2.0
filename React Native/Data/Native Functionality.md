---
tags:
  - React_Native
---

# Native Functionality

## PlacesList.js

> `useNavigation` with params: _PlaceDetails_ is a screen (component) that received placeId as param in the route prop
> `function PlaceDetails({ route, navigation }) {}`
> const selectedPlaceId = route.params.placeId;

```jsx
import { useNavigation } from "@react-navigation/native";
import { FlatList, StyleSheet, Text, View } from "react-native";

import { Colors } from "../../constants/colors";
import PlaceItem from "./PlaceItem";

function PlacesList({ places }) {
	const navigation = useNavigation();

	function selectPlaceHandler(id) {
		navigation.navigate("PlaceDetails", {
			placeId: id,
		});
	}

	if (!places || places.length === 0) {
		return (
			<View style={styles.fallbackContainer}>
				<Text style={styles.fallbackText}>
					No places added yet - start adding some!
				</Text>
			</View>
		);
	}

	return (
		<FlatList
			style={styles.list}
			data={places}
			keyExtractor={(item) => item.id}
			renderItem={({ item }) => (
				<PlaceItem place={item} onSelect={selectPlaceHandler} />
			)}
		/>
	);
}

export default PlacesList;

const styles = StyleSheet.create({
	list: {
		margin: 24,
	},
	fallbackContainer: {
		flex: 1,
		justifyContent: "center",
		alignItems: "center",
	},
	fallbackText: {
		fontSize: 16,
		color: Colors.primary200,
	},
});
```

## PlaceDetails.js

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

## AllPlaces.js

`useIsFocused` might want to render different content based on the current focus state of the screen

when changing screen components aren't recreated just put on stuck so the solution `isFocused` hook, so if you go back into the stuck the components are not recreated and things insiede the logic of useEffects aren't triggered

```jsx
import { useEffect, useState } from "react";
import { useIsFocused } from "@react-navigation/native";

import PlacesList from "../components/Places/PlacesList";
import { fetchPlaces } from "../util/database";

function AllPlaces({ route }) {
	const [loadedPlaces, setLoadedPlaces] = useState([]);

	const isFocused = useIsFocused();

	useEffect(() => {
		async function loadPlaces() {
			const places = await fetchPlaces();
			setLoadedPlaces(places);
		}

		if (isFocused) {
			loadPlaces();
			// setLoadedPlaces((curPlaces) => [...curPlaces, route.params.place]);
		}
	}, [isFocused]);

	return <PlacesList places={loadedPlaces} />;
}

export default AllPlaces;
```

---
