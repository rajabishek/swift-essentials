

- The `android` command opens the standalone android SDK manager application.
- Running the `android avd` command will open the AVD manager that is used to manage the virtual devices.
- Running `android list avd` will list all the existing virtual devices.
- Run the `emulator -avd <avd-name>` to launch the virtual device. Pressing ctrl+c will abort the process and sometimes curropt the ondisk storage of the virtual device. This isn't a disaster, you can wipe the storage clean and start over.
- Run the `adb devices` command to list the currently attached devices, i.e these are the devices in which you can launch the app.
- Running the `monitor` command will open the android device monitor tool.
- Running the command `android create project --target 1 --name SampleApplication --path ~/Desktop/SampleApplication --activity MainActivity --package com.rajabishek.sampleapplication` will create a new project call SampleApplication in the Desktop within a folder name called Samplepplication with a main activity called MainActivity with a package called `com.rajabishek.sampleapplication` with the 1st sdk image(target 1), we have anyway only 1 sdk image installed for API 24.


## Handling device orientations
When we change the orientation of the device the activity is destroyed and recreated. Now how do we listen to these orientation changes. In the app maifest files in the activity XML tag we have to add an attribute android:configChanges="orientation|screenSize", by doing this android will inform us whenever the orientation was changes. It informs us by calling the onConfigurationChanged method on the activity class. The code is shown below.
```java
@Override
public void onConfigurationChanged(Configuration newConfig) {
    super.onConfigurationChanged(newConfig);

    Log.d(MESSAGE_TAG, "The configuration was changed.");

    if(newConfig.orientation == Configuration.ORIENTATION_LANDSCAPE) {
        Log.d(MESSAGE_TAG, "The phone is in landscape.");
    } else {
        Log.d(MESSAGE_TAG, "The phone is in portrait.");
    }
}
```
Once we listen for orientation changes i.e once we add the android:configChanges="orientation|screenSize" in the activity XML tag the behavior of the activity is also changes, now no more the activity is completely destroyed and recreated. The activity survives any state change during the screen orientation change process. The activity state remains in the resumed state even though we change the orientation of the device. Now another thing that I would like to mention here is that if you to only maintain one screen orientation even though the user rotates the device, for example some games only work in landscape mode, even if the screen is rotated the game still is presented in landscape. To enable this you have to place an attribute in the  activity tag of the manifest file android:screenOrientation="landscape". Again by doing this since the activity's position need not change with respect to the device the activity state remains the same, there is not state transition and it remains in the resumes state.