---
tags:
  - React_Native
---

# app.json

> base config by expo for our app; then again is the place where write all config info.

```json
{
	"expo": {
		"name": "RNCourse",
		"slug": "RNCourse",
		"version": "1.0.0",
		"orientation": "portrait",
		"icon": "./assets/icon.png",
		"backgroundColor": "#24180f",
		"splash": {
			"image": "./assets/splash.png",
			"resizeMode": "contain",
			"backgroundColor": "#ffffff"
		},
		"updates": {
			"fallbackToCacheTimeout": 0
		},
		"assetBundlePatterns": ["**/*"],
		"ios": {
			"supportsTablet": true
		},
		"android": {
			"adaptiveIcon": {
				"foregroundImage": "./assets/adaptive-icon.png",
				"backgroundColor": "#FFFFFF"
			}
		},
		"web": {
			"favicon": "./assets/favicon.png"
		}
	}
}
```

---