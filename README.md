# UPDATE

Appium Android Driver issue is fixed in 1.6.x

Patched ChomeDriver binaries for macOS and Windows can be found here: https://github.com/blutter/appium-crosswalk-fix

# appium-android-xwalk-driver-patch

Appium issue: https://github.com/appium/appium/issues/4597

Patched chromdriver binary provided by https://github.com/piotrekkmt/chromedriver-appium

# Known issues

- With seems like with this patch it is possible to work only with NATIVE and base WEBVIEW contexts. I wasn't able yet to switch to second, third etc. opened webview within the app
- 'autoWebview' capability is not supported, you have to manualy switch app to webview after driver initialization

# How-to

This patch allows Appium to find Crosswalk (Cordova) webviews on Android

Appium: 1.5.3
Platform: macOS
Patch would work with any other OS as well but you'll need chromedriver binary for that OS 
Windows: https://github.com/ITKarel/ChromeDriver
Linux: https://github.com/chOOnz/xwalk-chromedriver

The steps as follows:

Install Appium 1.5.3 via NPM
```
$ npm install -g appium
```
Patch appium-android-driver
```
$ cd /usr/local/lib/node_modules/appium/node_modules/appium-android-driver/
$ patch -p1 lib/webview-helpers.js /path/to/patch/file/webview_helpers_cordova_suppport.patch
```
Rebuild Appium Android Driver (run command in module root directory)
```
$ npm install
```
Copy patched ChromeDriver into module directory
```
$ cd /usr/local/lib/node_modules/appium/node_modules/appium-android-driver/node_modules/appium-chromedriver/chromedriver/mac/
$ cp -i /path/to/patched/file/chromedriver .
overwrite ./chromedriver? (y/n [n]) y
```
Start Appium as usual
```
$ appium
[Appium] Welcome to Appium v1.5.3 (REV 02ccb7dcfd188d33795d19a2846c937c93851ff8)
[Appium] Appium REST http interface listener started on 0.0.0.0:4723
```
Add capabilities as follows

Java:
```
String ANDROID_APP_PACKAGE = "com.example.app.package";
String ANDROID_DEVICE_SOCKET = ANDROID_APP_PACKAGE + "_devtools_remote";
capabilities.setCapability("androidDeviceSocket", ANDROID_DEVICE_SOCKET);
ChromeOptions chromeOptions = new ChromeOptions();
chromeOptions.setExperimentalOption("androidDeviceSocket", ANDROID_DEVICE_SOCKET);
capabilities.setCapability(ChromeOptions.CAPABILITY, chromeOptions);
```
Ruby:
```
caps = {
    ...
    'androidDeviceSocket' => 'com.example.app.package_devtools_remote',
    'chromeOptions' => {
        'androidDeviceSocket' => 'com.example.app.package_devtools_remote'
    }
  }
```
