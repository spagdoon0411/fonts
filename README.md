## Overview

This repo contains a minimum reproducible example of an issue with custom fonts in Expo.

## Steps to Reproduce

1. Run

```bash
npx create-expo-app fonts
```

to create a new default project called `fonts`.

2. `cd` to the project directory and run

```bash
npm install expo-font
```

3. Create custom font files in the `assets/fonts` directory. This repository's assets/fonts
   directory contains the font `IBMPlexSansCond-Bold.ttf`, downloaded from [Google Fonts](https://fonts.google.com/specimen/IBM+Plex+Sans+Condensed?query=ibm+plex+sans+con).

4. Add the following `app.json` snippet under "expo" in `app.json`:

```json
"plugins": [
  [
    "expo-font",
    {
      "fonts": ["./assets/fonts/Inter-Black.otf"]
    }
  ]
]
```

This repostiory's `app.json` file is:

```json
{
  "expo": {
    "plugins": [
      [
        "expo-font",
        {
          "fonts": ["./assets/fonts/Inter-Black.otf"]
        }
      ]
    ],
    "name": "fonts",
    "slug": "fonts",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/icon.png",
    "userInterfaceStyle": "light",
    "splash": {
      "image": "./assets/splash.png",
      "resizeMode": "contain",
      "backgroundColor": "#ffffff"
    },
    "assetBundlePatterns": ["**/*"],
    "ios": {
      "supportsTablet": true
    },
    "android": {
      "adaptiveIcon": {
        "foregroundImage": "./assets/adaptive-icon.png",
        "backgroundColor": "#ffffff"
      }
    },
    "web": {
      "favicon": "./assets/favicon.png"
    }
  }
}
```

5. In 'App.js,' specify a new style so that the default styles object looks like this:

```javascript
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#fff",
    alignItems: "center",
    justifyContent: "center",
  },
  text: {
    color: "red",
    fontFamily: "IBMPlexSansCond-Bold",
  },
});
```

Note that the name of the font is its Postscript name. Apply this style to the default text so that
the App function looks like this:

```javascript
export default function App() {
  return (
    <View style={styles.container}>
      <Text style={styles.text}>
        Open up App.js to start working on your app!
      </Text>
      <StatusBar style="auto" />
    </View>
  );
}
```

6. Run the project with `npx expo start` and scan the QR code to open in Expo Go.

## Actual Behavior

The following error is logged

```text
 WARN  fontFamily "IBMPlexSansCond-Bold" is not a system font and has not been loaded through expo-font.
    at RCTText
    at Text (http://50zna-m-anonymous-8081.exp.direct/node_modules/expo/AppEntry.bundle//&platform=ios&dev=true&hot=false&transform.engine=hermes&transform.bytecode=true&transform.routerRoot=app:88805:27)
    at RCTView
    at View (http://50zna-m-anonymous-8081.exp.direct/node_modules/expo/AppEntry.bundle//&platform=ios&dev=true&hot=false&transform.engine=hermes&transform.bytecode=true&transform.routerRoot=app:70182:43)
    at App
    at withDevTools(App) (http://50zna-m-anonymous-8081.exp.direct/node_modules/expo/AppEntry.bundle//&platform=ios&dev=true&hot=false&transform.engine=hermes&transform.bytecode=true&transform.routerRoot=app:135340:27)
    at RCTView
    at View (http://50zna-m-anonymous-8081.exp.direct/node_modules/expo/AppEntry.bundle//&platform=ios&dev=true&hot=false&transform.engine=hermes&transform.bytecode=true&transform.routerRoot=app:70182:43)
    at RCTView
    at View (http://50zna-m-anonymous-8081.exp.direct/node_modules/expo/AppEntry.bundle//&platform=ios&dev=true&hot=false&transform.engine=hermes&transform.bytecode=true&transform.routerRoot=app:70182:43)
    at AppContainer (http://50zna-m-anonymous-8081.exp.direct/node_modules/expo/AppEntry.bundle//&platform=ios&dev=true&hot=false&transform.engine=hermes&transform.bytecode=true&transform.routerRoot=app:69993:36)
    at main(RootComponent) (http://50zna-m-anonymous-8081.exp.direct/node_modules/expo/AppEntry.bundle//&platform=ios&dev=true&hot=false&transform.engine=hermes&transform.bytecode=true&transform.routerRoot=app:117466:28)
```

The font color is red, indiciating that the style was applied--but that the font itself was not.

## Expected Behavior

The font should be loaded through `expo-font` and applied to the text, so that the text is red and in the IBM Plex Sans Condensed font.
