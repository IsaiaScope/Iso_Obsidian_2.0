---
tags: 
- Ionic
- Android_Studio
---

# Set Up Android Studio Up

I'm not sure if all of this steps are necessary but on the end of the road the app seems work fine

## npm package

```bash
npm install -g native-run
```

## Versioning

- Ionic 7
  - ANDROID SDK v33
  - Gradle Keep Version on Android Studio (Never Update Brakes Build)

To change ANDROID SDK version go to Android Studio - File - Language & Frameworks - SDK Tools - check the Show Package Details checkbox - select just the version you need under Android SDK Build-Tools

[[Set Up Android Studio - Versioning SDK.png]]

## Environment Variables

[Android Emulator Live reload - You Tube](https://www.youtube.com/watch?v=jJxRN_aWdi4)

[Ionic Doc v6 - Developing for Android](https://ionicframework.com/docs/v6/developing/android)

- Variables to add
  - ANDROID_SDK_ROOT
    - get path from Android Studio - File - Language & Frameworks - Android SDK - Android SDK Location
  - JAVA_HOME ()
    - get path from Android Studio - File - Build, Execution, Deployment - Build Tools - Gradle Gradle JDK
- Into Path
  - %ANDROID_SDK_ROOT%\tools\bin;
  - %ANDROID_SDK_ROOT%\platform-tools;%ANDROID_SDK_ROOT%\emulator;
  - %ANDROID_SDK_ROOT%\build-tools\33.0.0;
  - %JAVA_HOME%\bin;

---

## Good Resource For Errors

[Doc Android-Errors](https://github.com/ionic-team/native-run/wiki/Android-Errors)

---
