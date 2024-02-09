---
tags:
  - React_Native
---

# Auth

Basic firebase auth

## App.js

```jsx
import { useContext, useEffect, useState } from "react";
import { NavigationContainer } from "@react-navigation/native";
import { createNativeStackNavigator } from "@react-navigation/native-stack";
import { StatusBar } from "expo-status-bar";
import AsyncStorage from "@react-native-async-storage/async-storage";
import AppLoading from "expo-app-loading";

import LoginScreen from "./screens/LoginScreen";
import SignupScreen from "./screens/SignupScreen";
import WelcomeScreen from "./screens/WelcomeScreen";
import { Colors } from "./constants/styles";
import AuthContextProvider, { AuthContext } from "./store/auth-context";
import IconButton from "./components/ui/IconButton";

const Stack = createNativeStackNavigator();

function AuthStack() {
	return (
		<Stack.Navigator
			screenOptions={{
				headerStyle: { backgroundColor: Colors.primary500 },
				headerTintColor: "white",
				contentStyle: { backgroundColor: Colors.primary100 },
			}}
		>
			<Stack.Screen name="Login" component={LoginScreen} />
			<Stack.Screen name="Signup" component={SignupScreen} />
		</Stack.Navigator>
	);
}

function AuthenticatedStack() {
	const authCtx = useContext(AuthContext);
	return (
		<Stack.Navigator
			screenOptions={{
				headerStyle: { backgroundColor: Colors.primary500 },
				headerTintColor: "white",
				contentStyle: { backgroundColor: Colors.primary100 },
			}}
		>
			<Stack.Screen
				name="Welcome"
				component={WelcomeScreen}
				options={{
					headerRight: ({ tintColor }) => (
						<IconButton
							icon="exit"
							color={tintColor}
							size={24}
							onPress={authCtx.logout}
						/>
					),
				}}
			/>
		</Stack.Navigator>
	);
}

function Navigation() {
	const authCtx = useContext(AuthContext);

	return (
		<NavigationContainer>
			{!authCtx.isAuthenticated && <AuthStack />}
			{authCtx.isAuthenticated && <AuthenticatedStack />}
		</NavigationContainer>
	);
}

function Root() {
	const [isTryingLogin, setIsTryingLogin] = useState(true);

	const authCtx = useContext(AuthContext);

	useEffect(() => {
		async function fetchToken() {
			const storedToken = await AsyncStorage.getItem("token");

			if (storedToken) {
				authCtx.authenticate(storedToken);
			}

			setIsTryingLogin(false);
		}

		fetchToken();
	}, []);

	if (isTryingLogin) {
		return <AppLoading />;
	}

	return <Navigation />;
}

export default function App() {
	return (
		<>
			<StatusBar style="light" />
			<AuthContextProvider>
				<Root />
			</AuthContextProvider>
		</>
	);
}
```

---

## auth-context.js

`AsyncStorage` persist data in device

```jsx
import AsyncStorage from "@react-native-async-storage/async-storage";

import { createContext, useEffect, useState } from "react";

export const AuthContext = createContext({
	token: "",
	isAuthenticated: false,
	authenticate: (token) => {},
	logout: () => {},
});

function AuthContextProvider({ children }) {
	const [authToken, setAuthToken] = useState();

	function authenticate(token) {
		setAuthToken(token);
		AsyncStorage.setItem("token", token);
	}

	function logout() {
		setAuthToken(null);
		AsyncStorage.removeItem("token");
	}

	const value = {
		token: authToken,
		isAuthenticated: !!authToken,
		authenticate: authenticate,
		logout: logout,
	};

	return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
}

export default AuthContextProvider;
```

---

## AuthContent.js

auth form navigation example

```jsx
import { useState } from "react";
import { Alert, StyleSheet, View } from "react-native";
import { useNavigation } from "@react-navigation/native";

import FlatButton from "../ui/FlatButton";
import AuthForm from "./AuthForm";
import { Colors } from "../../constants/styles";

function AuthContent({ isLogin, onAuthenticate }) {
	const navigation = useNavigation();

	const [credentialsInvalid, setCredentialsInvalid] = useState({
		email: false,
		password: false,
		confirmEmail: false,
		confirmPassword: false,
	});

	function switchAuthModeHandler() {
		if (isLogin) {
			navigation.replace("Signup");
		} else {
			navigation.replace("Login");
		}
	}

	function submitHandler(credentials) {
		let { email, confirmEmail, password, confirmPassword } = credentials;

		email = email.trim();
		password = password.trim();

		const emailIsValid = email.includes("@");
		const passwordIsValid = password.length > 6;
		const emailsAreEqual = email === confirmEmail;
		const passwordsAreEqual = password === confirmPassword;

		if (
			!emailIsValid ||
			!passwordIsValid ||
			(!isLogin && (!emailsAreEqual || !passwordsAreEqual))
		) {
			Alert.alert("Invalid input", "Please check your entered credentials.");
			setCredentialsInvalid({
				email: !emailIsValid,
				confirmEmail: !emailIsValid || !emailsAreEqual,
				password: !passwordIsValid,
				confirmPassword: !passwordIsValid || !passwordsAreEqual,
			});
			return;
		}
		onAuthenticate({ email, password });
	}

	return (
		<View style={styles.authContent}>
			<AuthForm
				isLogin={isLogin}
				onSubmit={submitHandler}
				credentialsInvalid={credentialsInvalid}
			/>
			<View style={styles.buttons}>
				<FlatButton onPress={switchAuthModeHandler}>
					{isLogin ? "Create a new user" : "Log in instead"}
				</FlatButton>
			</View>
		</View>
	);
}

export default AuthContent;

const styles = StyleSheet.create({
	authContent: {
		marginTop: 64,
		marginHorizontal: 32,
		padding: 16,
		borderRadius: 8,
		backgroundColor: Colors.primary800,
		elevation: 2,
		shadowColor: "black",
		shadowOffset: { width: 1, height: 1 },
		shadowOpacity: 0.35,
		shadowRadius: 4,
	},
	buttons: {
		marginTop: 8,
	},
});
```

---

## AuthForm.js

auth form example

```jsx
import { useState } from "react";
import { StyleSheet, View } from "react-native";

import Button from "../ui/Button";
import Input from "./Input";

function AuthForm({ isLogin, onSubmit, credentialsInvalid }) {
	const [enteredEmail, setEnteredEmail] = useState("");
	const [enteredConfirmEmail, setEnteredConfirmEmail] = useState("");
	const [enteredPassword, setEnteredPassword] = useState("");
	const [enteredConfirmPassword, setEnteredConfirmPassword] = useState("");

	const {
		email: emailIsInvalid,
		confirmEmail: emailsDontMatch,
		password: passwordIsInvalid,
		confirmPassword: passwordsDontMatch,
	} = credentialsInvalid;

	function updateInputValueHandler(inputType, enteredValue) {
		switch (inputType) {
			case "email":
				setEnteredEmail(enteredValue);
				break;
			case "confirmEmail":
				setEnteredConfirmEmail(enteredValue);
				break;
			case "password":
				setEnteredPassword(enteredValue);
				break;
			case "confirmPassword":
				setEnteredConfirmPassword(enteredValue);
				break;
		}
	}

	function submitHandler() {
		onSubmit({
			email: enteredEmail,
			confirmEmail: enteredConfirmEmail,
			password: enteredPassword,
			confirmPassword: enteredConfirmPassword,
		});
	}

	return (
		<View style={styles.form}>
			<View>
				<Input
					label="Email Address"
					onUpdateValue={updateInputValueHandler.bind(this, "email")}
					value={enteredEmail}
					keyboardType="email-address"
					isInvalid={emailIsInvalid}
				/>
				{!isLogin && (
					<Input
						label="Confirm Email Address"
						onUpdateValue={updateInputValueHandler.bind(this, "confirmEmail")}
						value={enteredConfirmEmail}
						keyboardType="email-address"
						isInvalid={emailsDontMatch}
					/>
				)}
				<Input
					label="Password"
					onUpdateValue={updateInputValueHandler.bind(this, "password")}
					secure
					value={enteredPassword}
					isInvalid={passwordIsInvalid}
				/>
				{!isLogin && (
					<Input
						label="Confirm Password"
						onUpdateValue={updateInputValueHandler.bind(
							this,
							"confirmPassword"
						)}
						secure
						value={enteredConfirmPassword}
						isInvalid={passwordsDontMatch}
					/>
				)}
				<View style={styles.buttons}>
					<Button onPress={submitHandler}>
						{isLogin ? "Log In" : "Sign Up"}
					</Button>
				</View>
			</View>
		</View>
	);
}

export default AuthForm;

const styles = StyleSheet.create({
	buttons: {
		marginTop: 12,
	},
});
```

---
