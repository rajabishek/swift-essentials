

- The `android` command opens the standalone android SDK manager application.
- Running the `android avd` command will open the AVD manager that is used to manage the virtual devices.
- Running `android list avd` will list all the existing virtual devices.
- Run the `emulator -avd <avd-name>` to launch the virtual device. Pressing ctrl+c will abort the process and sometimes curropt the ondisk storage of the virtual device. This isn't a disaster, you can wipe the storage clean and start over.
- Run the `adb devices` command to list the currently attached devices, i.e these are the devices in which you can launch the app.
- Running the `monitor` command will open the android device monitor tool.
- Running the command `android create project --target 1 --name SampleApplication --path ~/Desktop/SampleApplication --activity MainActivity --package com.rajabishek.sampleapplication` will create a new project call SampleApplication in the Desktop within a folder name called Samplepplication with a main activity called MainActivity with a package called `com.rajabishek.sampleapplication` with the 1st sdk image(target 1), we have anyway only 1 sdk image installed for API 24.