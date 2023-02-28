# DocScan

A Flutter plugin that allows users to quickly snap pictures of documents and scan them for easy viewing and storage

## Usage

### iOS

iOS 10.0 or higher is needed to use the plugin. If compiling for any version lower than 10.0 make sure to check the iOS version before using the plugin. Change the minimum platform version to 10 (or higher) in your `ios/Podfile` file.

Add below permission to the `ios/Runner/Info.plist`:

Or in text format add the key:

```xml
<key>NSCameraUsageDescription</key>
<string>Can I use the camera please?</string>
```

** If you face any issue with the permission for newer ios versions, then add the followings to your `ios/Runner/Info.plist` file:

```Pod
post_install do |installer|
  installer.pods_project.targets.each do |target|
    flutter_additional_ios_build_settings(target)
    # Add below code
    target.build_configurations.each do |config|
       # Here are some configurations automatically generated by flutter
       # You can remove unused permissions here
       # for more information: https://github.com/BaseflowIT/flutter-permission-handler/blob/develop/permission_handler/ios/Classes/PermissionHandlerEnums.h
       # e.g. when you don't need camera permission, just add 'PERMISSION_CAMERA=0'
       config.build_settings['GCC_PREPROCESSOR_DEFINITIONS'] ||= [
         '$(inherited)',
         ## dart: PermissionGroup.camera
         'PERMISSION_CAMERA=1',
       ]
     end
     # finish code
  end
end
```

### Android

The plugin code is written in kotlin 1.5.31 so the same has to be set to the android project of yours for compilation.
Change the kotlin_version to 1.5.31 in your `android/build.gradle` file.

```gradle
ext.kotlin_version = '1.5.31'
```

Change the minimum Android SDK version to 21 (or higher) in your `android/app/build.gradle` file.

```
minSdkVersion 21
```

### Add dependency

Please check the latest version before installation.

```yaml
dependencies:
  flutter:
    sdk: flutter
  docscan: ^1.0.0
  path_provider: ^2.0.13
  permission_handler: ^10.2.0
  path: ^1.8.2 
```

### Add the following imports to your Dart code

```dart
import 'package:docscan/docscan.dart';
```

```dart

// Check permissions and request its
bool isCameraGranted = await Permission.camera.request().isGranted;
if (!isCameraGranted) {
    isCameraGranted = await Permission.camera.request() == PermissionStatus.granted;
}
if (!isCameraGranted) {
    // Have not permission to camera
    return;
}
// Use below code for live camera.
try {
      //Make sure to await the call to scan document
      List<String>? imgPaths = await DocScan.scanDocument();
      // If the widget was removed from the tree while the asynchronous platform
      // message was in flight, we want to discard the reply rather than calling
      // setState to update our non-existent appearance.
      if (!mounted) return;
      if (imgPaths == null || imgPaths.isEmpty) {
        return;
      }
      setState(() {
        _imagePath = imgPaths[0];
      });
    } catch (e) {
      print(e);
    }

```

### Acknowledgements

This plugin is based on the work of the following projects: edge_detection, cunning_document_scanner

### Author

[Md Zuhabul Islam](https:/github.com/zuhabul)

### Maintainer

[Hridoy](https://github.com/hr1d0y),
[Juwel](https://github.com/juwels1996)

### Publisher

[CrackTech Limited](https://cracktech.org)
