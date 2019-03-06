# rn-appcenter-expo
Just a sample React Native project created with create-react-native-app (with expo) and then ejected to add AppCenter. This project is generated to be able to re-create the issue we are facing.

The problem I'm still getting with the XCode build is: 'React/RCTDefines.h' file not found, so I'm not able to make it run.

I'm documented everything and it's matching with the commits I made in this repo so it's easy to replicate, so I'll appreciate any help with this issue so we can find the way to add AppCenter to a project after ejection from expo.

## Example

Environment:

- Mac: 10.14.3
- NPM: 6.8.0
- node: v10.15.2
- CocoaPods: v1.4.0
- XCode: 10.1 (10B61)

## Steps to get to this point

I tried to follow the [official guide](https://docs.microsoft.com/en-us/appcenter/sdk/getting-started/react-native) and a brand new project so all is cleaned.

- Create a new project with create-react-native-app

`create-react-native-app rn-appcenter-expo`

At this point all working normally

- Eject from expo (keeping expo kit)

`expo eject`

- Then we build both `ios` and `android`

`cd ios && pod install`
`cd ../android && ./gradlew installDevKernelDebug`

- Adding the needed string for `AppDelgate.m` for AppCenter linking process

  Add `NSURL *jsCodeLocation;` line to the `AppDelgate.m`

- installing AppCenter exact Pods

Add this exact Pods to the ios/Podfile

```
  pod 'AppCenter', '~> 1.13.2'
  pod 'AppCenter/Crashes', '~> 1.13.2'
  pod 'AppCenter/Push', '~> 1.13.2'
  pod 'AppCenter/Analytics', '~> 1.13.2'
  pod 'AppCenterReactNativeShared', '~> 1.12.2'
```

And then running `cd ios/ && pod repo update; pod install`

- Then we install app center npm dependencies

`cd ../ && npm install --save-exact appcenter appcenter-analytics appcenter-crashes appcenter-push`

- And finally being able to run link with no error:

`react-native link`

## Problem build `./ios/react-native-with-appcenter-after-having-ejected-expo--keeping-exposdk-.xcworkspace` on XCode

After all of that still I'm getting build failed, showing in the file:
`Pods>Development Pods>React>Core>Base>RCTAssert.h` has the problem `'React/RCTDefines.h' file not found` on line 10.

> I tried also one solution for this but still after cleaning the builds and re-runing the build, still the same `'React/RCTDefines.h' file not found` in the same file.

- On xCode then we need to make changes: (https://github.com/facebook/react-native/issues/12265)

  - Add React to Build Target before the app
  - Unchek parallelize build?

