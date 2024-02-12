---
tags:
  - React_Native
---

# Camera

## ImagePicker.js

imges picker ios needs permission to work, permission must be coded instead android has the allow modal by default

[`PermissionStatus` & `useCameraPermissions`](https://docs.expo.dev/versions/latest/sdk/imagepicker/#permissionstatus)

```jsx
import { Alert, Image, StyleSheet, Text, View } from "react-native";
import {
	launchCameraAsync,
	useCameraPermissions,
	PermissionStatus,
} from "expo-image-picker";
import { useState } from "react";

import { Colors } from "../../constants/colors";
import OutlinedButton from "../UI/OutlinedButton";

function ImagePicker({ onTakeImage }) {
	const [pickedImage, setPickedImage] = useState();

	const [
		cameraPermissionInformation,
		requestPermission,
	] = useCameraPermissions();

	async function verifyPermissions() {
		if (cameraPermissionInformation.status === PermissionStatus.UNDETERMINED) {
			const permissionResponse = await requestPermission();

			return permissionResponse.granted;
		}

		if (cameraPermissionInformation.status === PermissionStatus.DENIED) {
			Alert.alert(
				"Insufficient Permissions!",
				"You need to grant camera permissions to use this app."
			);
			return false;
		}

		return true;
	}

	async function takeImageHandler() {
		const hasPermission = await verifyPermissions();

		if (!hasPermission) {
			return;
		}

		const image = await launchCameraAsync({
			allowsEditing: true,
			aspect: [16, 9],
			quality: 0.5,
		});

		setPickedImage(image.uri);
		onTakeImage(image.uri);
	}

	let imagePreview = <Text>No image taken yet.</Text>;

	if (pickedImage) {
		imagePreview = <Image style={styles.image} source={{ uri: pickedImage }} />;
	}

	return (
		<View>
			<View style={styles.imagePreview}>{imagePreview}</View>
			<OutlinedButton icon="camera" onPress={takeImageHandler}>
				Take Image
			</OutlinedButton>
		</View>
	);
}

export default ImagePicker;

const styles = StyleSheet.create({
	imagePreview: {
		width: "100%",
		height: 200,
		marginVertical: 8,
		justifyContent: "center",
		alignItems: "center",
		backgroundColor: Colors.primary100,
		borderRadius: 4,
		overflow: "hidden",
	},
	image: {
		width: "100%",
		height: "100%",
	},
});
```

---

## PlaceForm.js

Form where `ImagePicker` is implemented

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
