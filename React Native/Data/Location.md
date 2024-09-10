---
tags:
  - React_Native
---

# Location

## LocationPicker.js

[`useForegroundPermissions` & `PermissionStatus`](https://docs.expo.dev/versions/latest/sdk/location/#permissionstatus)

```jsx
import { useEffect, useState } from "react";
import { Alert, View, StyleSheet, Image, Text } from "react-native";
import {
	getCurrentPositionAsync,
	useForegroundPermissions,
	PermissionStatus,
} from "expo-location";
import {
	useNavigation,
	useRoute,
	useIsFocused,
} from "@react-navigation/native";

import { Colors } from "../../constants/colors";
import OutlinedButton from "../UI/OutlinedButton";
import { getAddress, getMapPreview } from "../../util/location";

function LocationPicker({ onPickLocation }) {
	const [pickedLocation, setPickedLocation] = useState();
	const isFocused = useIsFocused();

	const navigation = useNavigation();
	const route = useRoute();

	const [
		locationPermissionInformation,
		requestPermission,
	] = useForegroundPermissions();

	useEffect(() => {
		if (isFocused && route.params) {
			const mapPickedLocation = {
				lat: route.params.pickedLat,
				lng: route.params.pickedLng,
			};
			setPickedLocation(mapPickedLocation);
		}
	}, [route, isFocused]);

	useEffect(() => {
		async function handleLocation() {
			if (pickedLocation) {
				const address = await getAddress(
					pickedLocation.lat,
					pickedLocation.lng
				);
				onPickLocation({ ...pickedLocation, address: address });
			}
		}

		handleLocation();
	}, [pickedLocation, onPickLocation]);

	async function verifyPermissions() {
		if (
			locationPermissionInformation.status === PermissionStatus.UNDETERMINED
		) {
			const permissionResponse = await requestPermission();

			return permissionResponse.granted;
		}

		if (locationPermissionInformation.status === PermissionStatus.DENIED) {
			Alert.alert(
				"Insufficient Permissions!",
				"You need to grant location permissions to use this app."
			);
			return false;
		}

		return true;
	}

	async function getLocationHandler() {
		const hasPermission = await verifyPermissions();

		if (!hasPermission) {
			return;
		}

		const location = await getCurrentPositionAsync();
		setPickedLocation({
			lat: location.coords.latitude,
			lng: location.coords.longitude,
		});
	}

	function pickOnMapHandler() {
		navigation.navigate("Map");
	}

	let locationPreview = <Text>No location picked yet.</Text>;

	if (pickedLocation) {
		locationPreview = (
			<Image
				style={styles.image}
				source={{
					uri: getMapPreview(pickedLocation.lat, pickedLocation.lng),
				}}
			/>
		);
	}

	return (
		<View>
			<View style={styles.mapPreview}>{locationPreview}</View>
			<View style={styles.actions}>
				<OutlinedButton icon="location" onPress={getLocationHandler}>
					Locate User
				</OutlinedButton>
				<OutlinedButton icon="map" onPress={pickOnMapHandler}>
					Pick on Map
				</OutlinedButton>
			</View>
		</View>
	);
}

export default LocationPicker;

const styles = StyleSheet.create({
	mapPreview: {
		width: "100%",
		height: 200,
		marginVertical: 8,
		justifyContent: "center",
		alignItems: "center",
		backgroundColor: Colors.primary100,
		borderRadius: 4,
		overflow: "hidden",
	},
	actions: {
		flexDirection: "row",
		justifyContent: "space-around",
		alignItems: "center",
	},
	image: {
		width: "100%",
		height: "100%",
		// borderRadius: 4
	},
});
```

---

## PlaceForm.js

Form where `LocationPicker` is implemented

```jsx
import { useCallback, useState } from "react";
import { ScrollView, StyleSheet, Text, TextInput, View } from "react-native";

import { Colors } from "../../constants/colors";
import { Place } from "../../models/place";
import Button from "../UI/Button";
import ImagePicker from "./ImagePicker";
import LocationPicker from "./LocationPicker";

function PlaceForm({ onCreatePlace }) {
	const [enteredTitle, setEnteredTitle] = useState("");
	const [selectedImage, setSelectedImage] = useState();
	const [pickedLocation, setPickedLocation] = useState();

	function changeTitleHandler(enteredText) {
		setEnteredTitle(enteredText);
	}

	function takeImageHandler(imageUri) {
		setSelectedImage(imageUri);
	}

	const pickLocationHandler = useCallback((location) => {
		setPickedLocation(location);
	}, []);

	function savePlaceHandler() {
		const placeData = new Place(enteredTitle, selectedImage, pickedLocation);
		onCreatePlace(placeData);
	}

	return (
		<ScrollView style={styles.form}>
			<View>
				<Text style={styles.label}>Title</Text>
				<TextInput
					style={styles.input}
					onChangeText={changeTitleHandler}
					value={enteredTitle}
				/>
			</View>
			<ImagePicker onTakeImage={takeImageHandler} />
			<LocationPicker onPickLocation={pickLocationHandler} />
			<Button onPress={savePlaceHandler}>Add Place</Button>
		</ScrollView>
	);
}

export default PlaceForm;

const styles = StyleSheet.create({
	form: {
		flex: 1,
		padding: 24,
	},
	label: {
		fontWeight: "bold",
		marginBottom: 4,
		color: Colors.primary500,
	},
	input: {
		marginVertical: 8,
		paddingHorizontal: 4,
		paddingVertical: 8,
		fontSize: 16,
		borderBottomColor: Colors.primary700,
		borderBottomWidth: 2,
		backgroundColor: Colors.primary100,
	},
});
```

---

## location.js

API key must be create on google services

```jsx
const GOOGLE_API_KEY = "AIzaSyD2mP_sDlloOcwv6OLmYtATOJIcnDhcZcg";

export function getMapPreview(lat, lng) {
	const imagePreviewUrl = `https://maps.googleapis.com/maps/api/staticmap?center=${lat},${lng}&zoom=14&size=400x200&maptype=roadmap&markers=color:red%7Clabel:S%7C${lat},${lng}&key=${GOOGLE_API_KEY}`;
	return imagePreviewUrl;
}

export async function getAddress(lat, lng) {
	const url = `https://maps.googleapis.com/maps/api/geocode/json?latlng=${lat},${lng}&key=${GOOGLE_API_KEY}`;
	const response = await fetch(url);

	if (!response.ok) {
		throw new Error("Failed to fetch address!");
	}

	const data = await response.json();
	const address = data.results[0].formatted_address;
	return address;
}
```

---
