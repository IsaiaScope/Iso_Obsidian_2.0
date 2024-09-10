---
tags:
  - React_Native
---

# Guess Number App

An portion of an app with multiple screen to guess a number

## App.js

- Expo
  - `useFonts` [:BoBxLink:](https://docs.expo.dev/versions/latest/sdk/font/); useFonts must be in root component
  - `LinearGradient` provides a native React view that transitions between multiple colors in a linear direction.
  - `AppLoading` expo-app-loading tells expo-splash-screen to keep the splash screen visible while the AppLoading component is mounted
  - `StatusBar` to control the app's status bar. The status bar is the zone, typically at the top of the screen, that displays the current time, Wi-Fi and cellular network information, battery level and/or other status icons
- React Native
  - `ImageBackground` component, which has the same props as `<Image>`, and add whatever children to it you would like to layer on top of it.
  - `SafeAreaView` renders nested content and automatically applies padding to reflect the portion of the view that is not covered by navigation bars, tab bars, toolbars, and other ancestor views.

```jsx
import { useState } from "react";
import { StyleSheet, ImageBackground, SafeAreaView } from "react-native";
import { LinearGradient } from "expo-linear-gradient";
import { useFonts } from "expo-font";
import AppLoading from "expo-app-loading";
import { StatusBar } from "expo-status-bar";

import StartGameScreen from "./screens/StartGameScreen";
import GameScreen from "./screens/GameScreen";
import GameOverScreen from "./screens/GameOverScreen";
import Colors from "./constants/colors";

export default function App() {
	const [userNumber, setUserNumber] = useState();
	const [gameIsOver, setGameIsOver] = useState(true);
	const [guessRounds, setGuessRounds] = useState(0);

	const [fontsLoaded] = useFonts({
		"open-sans": require("./assets/fonts/OpenSans-Regular.ttf"),
		"open-sans-bold": require("./assets/fonts/OpenSans-Bold.ttf"),
	});

	if (!fontsLoaded) {
		return <AppLoading />;
	}

	function pickedNumberHandler(pickedNumber) {
		setUserNumber(pickedNumber);
		setGameIsOver(false);
	}

	function gameOverHandler(numberOfRounds) {
		setGameIsOver(true);
		setGuessRounds(numberOfRounds);
	}

	function startNewGameHandler() {
		setUserNumber(null);
		setGuessRounds(0);
	}

	let screen = <StartGameScreen onPickNumber={pickedNumberHandler} />;

	if (userNumber) {
		screen = (
			<GameScreen userNumber={userNumber} onGameOver={gameOverHandler} />
		);
	}

	if (gameIsOver && userNumber) {
		screen = (
			<GameOverScreen
				userNumber={userNumber}
				roundsNumber={guessRounds}
				onStartNewGame={startNewGameHandler}
			/>
		);
	}

	return (
		<>
			<StatusBar style="light" />
			<LinearGradient
				colors={[Colors.primary700, Colors.accent500]}
				style={styles.rootScreen}
			>
				<ImageBackground
					source={require("./assets/images/background.png")}
					resizeMode="cover"
					style={styles.rootScreen}
					imageStyle={styles.backgroundImage}
				>
					<SafeAreaView style={styles.rootScreen}>{screen}</SafeAreaView>
				</ImageBackground>
			</LinearGradient>
		</>
	);
}

const styles = StyleSheet.create({
	rootScreen: {
		flex: 1,
	},
	backgroundImage: {
		opacity: 0.15,
	},
});
```

---

## PrimaryButton.js

- good approach to create a custom button
- `Pressable` is a Core Component wrapper that can detect various stages of press interactions on any of its defined children.

```jsx
import { View, Text, Pressable, StyleSheet } from "react-native";

import Colors from "../../constants/colors";

function PrimaryButton({ children, onPress }) {
	return (
		<View style={styles.buttonOuterContainer}>
			<Pressable
				style={({ pressed }) =>
					pressed
						? [styles.buttonInnerContainer, styles.pressed]
						: styles.buttonInnerContainer
				}
				onPress={onPress}
				android_ripple={{ color: Colors.primary600 }}
			>
				<Text style={styles.buttonText}>{children}</Text>
			</Pressable>
		</View>
	);
}

export default PrimaryButton;

const styles = StyleSheet.create({
	buttonOuterContainer: {
		borderRadius: 28,
		margin: 4,
		overflow: "hidden",
	},
	buttonInnerContainer: {
		backgroundColor: Colors.primary500,
		paddingVertical: 8,
		paddingHorizontal: 16,
		elevation: 2,
	},
	buttonText: {
		color: "white",
		textAlign: "center",
	},
	pressed: {
		opacity: 0.75,
	},
});
```

---

## StartGameScreen.js

- `Alert` Launches an alert dialog with the specified title and message.
- `KeyboardAvoidingView`This component will automatically adjust its height, position, or bottom padding based on the keyboard height to remain visible while the virtual keyboard is displayed.

Optionally provide a list of buttons. Tapping any button will fire the respective onPress callback and dismiss the alert. By default, the only button will be an 'OK' button.

```jsx
import { useState } from "react";
import {
	TextInput,
	View,
	StyleSheet,
	Alert,
	useWindowDimensions,
	KeyboardAvoidingView,
	ScrollView,
} from "react-native";

import PrimaryButton from "../components/ui/PrimaryButton";
import Title from "../components/ui/Title";
import Colors from "../constants/colors";
import Card from "../components/ui/Card";
import InstructionText from "../components/ui/InstructionText";

function StartGameScreen({ onPickNumber }) {
	const [enteredNumber, setEnteredNumber] = useState("");

	const { width, height } = useWindowDimensions();

	function numberInputHandler(enteredText) {
		setEnteredNumber(enteredText);
	}

	function resetInputHandler() {
		setEnteredNumber("");
	}

	function confirmInputHandler() {
		const chosenNumber = parseInt(enteredNumber);

		if (isNaN(chosenNumber) || chosenNumber <= 0 || chosenNumber > 99) {
			Alert.alert(
				"Invalid number!",
				"Number has to be a number between 1 and 99.",
				[{ text: "Okay", style: "destructive", onPress: resetInputHandler }]
			);
			return;
		}

		onPickNumber(chosenNumber);
	}

	const marginTopDistance = height < 380 ? 30 : 100;

	return (
		<ScrollView style={styles.screen}>
			<KeyboardAvoidingView style={styles.screen} behavior="position">
				<View style={[styles.rootContainer, { marginTop: marginTopDistance }]}>
					<Title>Guess My Number</Title>
					<Card>
						<InstructionText>Enter a Number</InstructionText>
						<TextInput
							style={styles.numberInput}
							maxLength={2}
							keyboardType="number-pad"
							autoCapitalize="none"
							autoCorrect={false}
							onChangeText={numberInputHandler}
							value={enteredNumber}
						/>
						<View style={styles.buttonsContainer}>
							<View style={styles.buttonContainer}>
								<PrimaryButton onPress={resetInputHandler}>Reset</PrimaryButton>
							</View>
							<View style={styles.buttonContainer}>
								<PrimaryButton onPress={confirmInputHandler}>
									Confirm
								</PrimaryButton>
							</View>
						</View>
					</Card>
				</View>
			</KeyboardAvoidingView>
		</ScrollView>
	);
}

export default StartGameScreen;

// const deviceHeight = Dimensions.get('window').height;

const styles = StyleSheet.create({
	screen: {
		flex: 1,
	},
	rootContainer: {
		flex: 1,
		// marginTop: deviceHeight < 380 ? 30 : 100,
		alignItems: "center",
	},
	numberInput: {
		height: 50,
		width: 50,
		fontSize: 32,
		borderBottomColor: Colors.accent500,
		borderBottomWidth: 2,
		color: Colors.accent500,
		marginVertical: 8,
		fontWeight: "bold",
		textAlign: "center",
	},
	buttonsContainer: {
		flexDirection: "row",
	},
	buttonContainer: {
		flex: 1,
	},
});
```

---

## GameScreen.js

- `Ionicons` [:BoBxLink:](https://docs.expo.dev/guides/icons/#expovector-icons)

```jsx
import { useState, useEffect } from "react";
import {
	View,
	StyleSheet,
	Alert,
	FlatList,
	useWindowDimensions,
} from "react-native";
import { Ionicons } from "@expo/vector-icons";

import NumberContainer from "../components/game/NumberContainer";
import Card from "../components/ui/Card";
import InstructionText from "../components/ui/InstructionText";
import PrimaryButton from "../components/ui/PrimaryButton";
import Title from "../components/ui/Title";
import GuessLogItem from "../components/game/GuessLogItem";

function generateRandomBetween(min, max, exclude) {
	const rndNum = Math.floor(Math.random() * (max - min)) + min;

	if (rndNum === exclude) {
		return generateRandomBetween(min, max, exclude);
	} else {
		return rndNum;
	}
}

let minBoundary = 1;
let maxBoundary = 100;

function GameScreen({ userNumber, onGameOver }) {
	const initialGuess = generateRandomBetween(1, 100, userNumber);
	const [currentGuess, setCurrentGuess] = useState(initialGuess);
	const [guessRounds, setGuessRounds] = useState([initialGuess]);
	const { width, height } = useWindowDimensions();

	useEffect(() => {
		if (currentGuess === userNumber) {
			onGameOver(guessRounds.length);
		}
	}, [currentGuess, userNumber, onGameOver]);

	useEffect(() => {
		minBoundary = 1;
		maxBoundary = 100;
	}, []);

	function nextGuessHandler(direction) {
		// direction => 'lower', 'greater'
		if (
			(direction === "lower" && currentGuess < userNumber) ||
			(direction === "greater" && currentGuess > userNumber)
		) {
			Alert.alert("Don't lie!", "You know that this is wrong...", [
				{ text: "Sorry!", style: "cancel" },
			]);
			return;
		}

		if (direction === "lower") {
			maxBoundary = currentGuess;
		} else {
			minBoundary = currentGuess + 1;
		}

		const newRndNumber = generateRandomBetween(
			minBoundary,
			maxBoundary,
			currentGuess
		);
		setCurrentGuess(newRndNumber);
		setGuessRounds((prevGuessRounds) => [newRndNumber, ...prevGuessRounds]);
	}

	const guessRoundsListLength = guessRounds.length;

	let content = (
		<>
			<NumberContainer>{currentGuess}</NumberContainer>
			<Card>
				<InstructionText style={styles.instructionText}>
					Higher or lower?
				</InstructionText>
				<View style={styles.buttonsContainer}>
					<View style={styles.buttonContainer}>
						<PrimaryButton onPress={nextGuessHandler.bind(this, "lower")}>
							<Ionicons name="md-remove" size={24} color="white" />
						</PrimaryButton>
					</View>
					<View style={styles.buttonContainer}>
						<PrimaryButton onPress={nextGuessHandler.bind(this, "greater")}>
							<Ionicons name="md-add" size={24} color="white" />
						</PrimaryButton>
					</View>
				</View>
			</Card>
		</>
	);

	if (width > 500) {
		content = (
			<>
				<View style={styles.buttonsContainerWide}>
					<View style={styles.buttonContainer}>
						<PrimaryButton onPress={nextGuessHandler.bind(this, "lower")}>
							<Ionicons name="md-remove" size={24} color="white" />
						</PrimaryButton>
					</View>
					<NumberContainer>{currentGuess}</NumberContainer>
					<View style={styles.buttonContainer}>
						<PrimaryButton onPress={nextGuessHandler.bind(this, "greater")}>
							<Ionicons name="md-add" size={24} color="white" />
						</PrimaryButton>
					</View>
				</View>
			</>
		);
	}

	return (
		<View style={styles.screen}>
			<Title>Opponent's Guess</Title>
			{content}
			<View style={styles.listContainer}>
				{/* {guessRounds.map(guessRound => <Text key={guessRound}>{guessRound}</Text>)} */}
				<FlatList
					data={guessRounds}
					renderItem={(itemData) => (
						<GuessLogItem
							roundNumber={guessRoundsListLength - itemData.index}
							guess={itemData.item}
						/>
					)}
					keyExtractor={(item) => item}
				/>
			</View>
		</View>
	);
}

export default GameScreen;

const styles = StyleSheet.create({
	screen: {
		flex: 1,
		padding: 24,
		alignItems: "center",
	},
	instructionText: {
		marginBottom: 12,
	},
	buttonsContainer: {
		flexDirection: "row",
	},
	buttonContainer: {
		flex: 1,
	},
	buttonsContainerWide: {
		flexDirection: "row",
		alignItems: "center",
	},
	listContainer: {
		flex: 1,
		padding: 16,
	},
});
```

---
