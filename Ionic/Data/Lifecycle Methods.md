---
tags: 
- Ionic
---

# Ionic Lifecycle Methods

> [lifecycle](https://ionicframework.com/docs/react/lifecycle)

Below are some tips on use cases for each of the life cycle events.

- `ionViewWillEnter` - Since `ionViewWillEnter` is called every time the view is navigated to (regardless if initialized or not), it's a good method to load data from services.
- `ionViewDidEnter` - If you see performance problems from using `ionViewWillEnter` when loading data, you can do your data calls in `ionViewDidEnter` instead. This event won't fire until after the page is visible to the user, however, so you might want to use either a loading indicator or a skeleton screen, so content doesn't flash in un-naturally after the transition is complete.
- `ionViewWillLeave` - Can be used for cleanup, like unsubscribing from data sources. Since `componentWillUnmount` might not fire when you navigate from the current page, put your cleanup code here if you don't want it active while the screen is not in view.
- `ionViewDidLeave` - When this event fires, you know the new page has fully transitioned in, so any logic you might not normally do when the view is visible can go here.

---

| Event Name         | Description                                                        |
| ------------------ | ------------------------------------------------------------------ |
| `ionViewWillEnter` | Fired when the component routing to is about to animate into view. |
| `ionViewDidEnter`  | Fired when the component routing to has *finished* animating.      |
| `ionViewWillLeave` | Fired when the component routing *from* is about to animate.       |
| `ionViewDidLeave`  | Fired when the component routing *from* has *finished* animating.  |

---

```jsx
import {
	IonContent,
	IonHeader,
	IonTitle,
	IonToolbar,
	useIonViewDidEnter,
	useIonViewDidLeave,
	useIonViewWillEnter,
	useIonViewWillLeave,
} from "@ionic/react";
import React from "react";

const HomePage: React.FC = () => {
	useIonViewDidEnter(() => {
		console.log("ionViewDidEnter event fired");
	});

	useIonViewDidLeave(() => {
		console.log("ionViewDidLeave event fired");
	});

	useIonViewWillEnter(() => {
		console.log("ionViewWillEnter event fired");
	});

	useIonViewWillLeave(() => {
		console.log("ionViewWillLeave event fired");
	});

	return (
		<IonPage>
			<IonHeader>
				<IonToolbar>
					<IonTitle>Home</IonTitle>
				</IonToolbar>
			</IonHeader>
			<IonContent></IonContent>
		</IonPage>
	);
};

export default HomePage;
```

---
