# BLECtrl (Bluetooth Low Energy Controller)

<img src="https://slavce.sj14apps.com/img/projects/BLECtrl.png" width="300" />

BLECtrl is an Android library that wraps the platform's Bluetooth Low Energy (BLE) APIs so you can scan for, connect to, and communicate with BLE devices without dealing with the low-level boilerplate yourself.

[![](https://jitpack.io/v/SlaVcE14/BLECtrl.svg)](https://jitpack.io/#SlaVcE14/BLECtrl)

## Requirements

- Android Studio with a recent Android Gradle Plugin
- JDK 17
- A device with Bluetooth Low Energy support (BLE does not work on most emulators)

## Installation

### 1. Add the JitPack repository

In your root `settings.gradle.kts`:

```kotlin
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
        maven { url = uri("https://jitpack.io") }
    }
}
```

If you still use `build.gradle` (Groovy) with `allprojects`:

```groovy
allprojects {
    repositories {
        google()
        mavenCentral()
        maven { url 'https://jitpack.io' }
    }
}
```

### 2. Add the dependency

In your app module's `build.gradle.kts`:

```kotlin
dependencies {
    implementation("com.github.SlaVcE14:BLECtrl:<version>")
}
```

Replace `<version>` with a release tag, a commit hash, or `master-SNAPSHOT` to track the latest commit. See the [JitPack page](https://jitpack.io/#SlaVcE14/BLECtrl) for available versions.

## Permissions

Declare the Bluetooth permissions your app needs in `AndroidManifest.xml`:

```xml
<!-- Android 12 (API 31) and above -->
<uses-permission android:name="android.permission.BLUETOOTH_SCAN"
    android:usesPermissionFlags="neverForLocation" />
<uses-permission android:name="android.permission.BLUETOOTH_CONNECT" />

<!-- Android 11 (API 30) and below -->
<uses-permission android:name="android.permission.BLUETOOTH"
    android:maxSdkVersion="30" />
<uses-permission android:name="android.permission.BLUETOOTH_ADMIN"
    android:maxSdkVersion="30" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"
    android:maxSdkVersion="30" />

<uses-feature android:name="android.hardware.bluetooth_le" android:required="true" />
```

You must also request the runtime permissions before scanning or connecting.

## Usage

```java
// Create controller
BluetoothController bluetoothController = new BluetoothController(bleCallback, checkPermissionCallBack);
bluetoothController.setupBluetooth();

// Callbacks
BluetoothController.CallBack bleCallback = new BluetoothController.CallBack() {
        @Override
        public void onConnect() {
            // Do something
        }

        @Override
        public void onDisconnect() {
            // Do something
        }

        @Override
        public void onStatusUpdate(BluetoothStatus status) {
            // Do something

            // Handle specific statuses
            // available status value: CONNECTING, CONNECTED, DISCONNECTED, RECEIVE, SCANNING, FAILED_SCANNING, SCAN_FINISHED, SEND, ERROR
            switch (status.status) {
                case SCAN_FINISHED:
                    // Do something
                    break;
                case CONNECTING:
                    // Do something
                    break;
                case ERROR:
                    // Do something
                    break;
            }
        }

        @Override
        public void onDataReceived(String data) {
            // Do something
        }
};

BluetoothController.CheckPermission checkPermissionCallBack = new BluetoothController.CheckPermission() {
        @Override
        public boolean hasBluetoothPermission() {
            // Check if has Bluetooth Permission
        }

        @Override
        public boolean hasScanPermission() {
            // Check if has Scan Permission
        }
};

BluetoothController.OnDeviceLoad onDeviceLoadCallBack = () -> {
        // Do something
        // updateDeviceList(bluetoothController.getDevices());
 };

// Register Discovery Receiver
bluetoothController.registerDiscoveryReceiver(context);

// Unegister Discovery Receiver
bluetoothController.unregisterDiscoveryReceiver(context);

// Load pared devices
bluetoothController.loadPairedDevices(onDeviceLoadCallBack);

//Scan for devices
bluetoothController.scanDevices(context);

// Get devices list
ArrayList<BluetoothDevice> devices = bluetoothController.getDevices();

// Get human-readable name of a device
String name = bluetoothController.getDeviceName(bluetoothDevice);

// Connect to a device
bluetoothController.selectDevice(bluetoothDevice);
bluetoothController.connectDevice(context);

// Send data
bluetoothController.sendData(string);

// Disconnect
bluetoothController.disconnect();
```

## Building from source

```bash
git clone https://github.com/SlaVcE14/BLECtrl.git
cd BLECtrl
./gradlew build
```

On Windows, use `gradlew.bat build`.

To publish to your local Maven repository for testing in another project (if the module is configured for it):

```bash
./gradlew publishToMavenLocal
```

## License
```
MIT License

Copyright (c) 2026 SlaVcE

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
