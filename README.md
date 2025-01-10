# ADB-KILLSWITCH
A killswitch for Android.

## INDEX <a name="INDEX"></a>

[COMMAND BREAKDOWN](#COMMANDBREAKDOWN)

[IMPORTANT INFORMATION](#IMPORTANTINFORMATION)

[CONFIGURATION](#CONFIGURATION)

[TODO](#TODO)

## COMMAND BREAKDOWN <a name="COMMANDBREAKDOWN"></a>

**ADB Killswitch achieves the following:**

- Disables Wi-Fi.
- Disables Mobile Data.
- Disables both SIM card slots.
- Disables Bluetooth.
- Disables NFC.
- Disables Location Services (GPS).
- Closes running apps (limited to 10 apps, but you can close more by adding to the command.)
- Uninstalls all non-system apps and system apps of your preference.
- Deletes all files and folders in internal storage (shared storage).
- Deletes all files and folders in an external SD card, if one is present. 
- Enables/changes a PIN password.
- Reboots your phone into Recovery Mode, which will give you the option to factory reset your device and wipe all data. 

**THE FULL COMMAND:**
```
svc wifi disable; svc data disable; svc phone disable 1; svc phone disable 2; settings put global bluetooth_disabled_profiles 0; svc nfc disable; settings put secure location_mode 0; input keyevent KEYCODE_APP_SWITCH; input keyevent 20; input keyevent DEL; input keyevent DEL; input keyevent DEL; input keyevent DEL; input keyevent DEL; input keyevent DEL; input keyevent DEL; input keyevent DEL; input keyevent DEL; input keyevent DEL; pm uninstall --user 0 <package name (e.g., "com.android.phone")>; rm -rf /storage/emulated/0/*; rm -rf /storage/<SD card ID>/*; cmd lock_settings set-pin --old <your old PIN number/any PIN number if you didn't originally have one> <new PIN number (4-16 digits)>; reboot recovery
```
**DISABLE WI-FI:**
```
svc wifi disable
```
**DISABLE MOBILE DATA:**
```
svc data disable
```
**DISABLE SIM CARD SLOTS:**
```
svc phone disable 1; svc phone disable 2
```
**DISABLE BLUETOOTH:**
```
settings put global bluetooth_disabled_profiles 0
```
**DISABLE NFC:**
```
svc nfc disable
```
**DISABLE LOCATION SERVICES:**
```
settings put secure location_mode 0
```
**CLOSE APPS:** 
```
input keyevent KEYCODE_APP_SWITCH; input keyevent 20; input keyevent DEL; input keyevent DEL; input keyevent DEL; input keyevent DEL; input keyevent DEL; input keyevent DEL; input keyevent DEL; input keyevent DEL; input keyevent DEL; input keyevent DEL
```
**UNINSTALL APPS:**
```
pm uninstall --user 0 <package name (e.g., "com.android.phone")>
```
**DELETE EVERYTHING IN INTERNAL STORAGE:**
```
rm -rf /storage/emulated/0/*
```
**DELETE EVERYTHING IN EXTERNAL SD CARD:**
```
rm -rf /storage/<SD card ID>/*
```
**ENABLE/CHANGE A PIN PASSWORD:**
```
cmd lock_settings set-pin --old <your old PIN number/any PIN number if you didn't originally have one> <new PIN number (4-16 digits)>
```
**REBOOT INTO RECOVERY MODE:**
```
reboot recovery
```

[ꜛBACK TO INDEXꜛ](#INDEX)

## IMPORTANT INFORMATION <a name="IMPORTANTINFORMATION"></a>

- You will need a computer and an Android device.

- If your device is rooted, you can skip instructions regarding using the Shizuku app.

- This killswitch can be executed without an internet connection.

- You can configure this command however you’d like, for any custom use-case. Don't worry if you don't want to configure everything provided here, just remove the segments of the command you don't want.

- Anything inside of "<>" must be configured with the correct information, then delete the brackets when done. 

- Each command must be separated by a semi-colon ";".

- Ensure nothing is misspelled in the command and that the format is correct. If there are any typos or incorrect formatting, the command will **NOT** work properly.

- If you shutdown/restart/reboot your Android device, you will lose the elevated status provided by Shizuku. This means that you won't be able to execute this command. You will have to run the Shizuku command on your computer again to regain superuser privileges.

- TEST THIS COMMAND THOROUGHLY BEFORE UTILIZING IN A REAL-LIFE SCENARIO. Androids are all different, even if just slightly. What works on one device may not work perfectly on another. Thankfully, it is extremely easy to test this command using either the aShell app with elevated privileges on your Android device or ADB on your computer.

- Store your command somewhere safe. When needed, copy and paste the command into aShell, then execute it. Keep in mind, once you execute this command, there is no way to undo its effects.

[ꜛBACK TO INDEXꜛ](#INDEX)

## CONFIGURATION <a name="CONFIGURATION"></a>

- **On your Android device, install [aShell](https://gitlab.com/sunilpaulmathew/ashell/-/releases), an ADB shell emulator app that will provide you a platform for executing ADB commands from your Android device.**

- **On your Android device, install [Shizuku](https://github.com/RikkaApps/Shizuku/releases), an app that will grant you superuser privileges without having to root your device, which is necessary for executing ADB commands without a computer. You will need a computer to initiate Shizuku once.**

- **On your computer, install and setup [ADB (Android Debug Bridge)](https://developer.android.com/tools/adb).**

- **Run the ADB daemon on your computer using this command:**
```
adb devices
```
You should receive a prompt on your Android device to allow USB Debugging from the specified source, press the button that approves this process. 

- **Disable ADB authorization timeout using this command:**
```
adb shell settings put global adb_authority_timeout 0
```
This will prevent ADB on your computer from timing out while you're trying to configure everything, or explore ADB's many commands and features.

- **Open the Shizuku app on your Android device and execute the command provided below on to your computer. This will run the Shizuku server which will elevate your user status to superuser, allowing you to execute ADB commands from your Android device instead of having to use a computer.**
```
adb shell sh /storage/emulated/0/Android/data/moe.shizuku.privileged.api/start.sh
```

- **Uninstalling all user-installed apps is key to reducing your attack surface. On your computer, use this ADB command to get a list of all user-installed apps:**
```
adb shell pm list packages -3
```
To list all packages (including system apps) installed on your device, use this command:
```
adb shell pm list packages
```

- **Input the names of the packages (apps) you wish to delete. The branch of the command looks like this:**
```
adb shell pm uninstall --user 0 <package name (e.g., "com.android.phone")> <any other packages you wish to uninstall following the same format of "; adb shell pm uninstall --user 0 <package name>;">
```

*WARNING: REMOVING ESSENTIAL SYSTEM APPS CAN INTERFERE WITH THE FACTORY RESET PROCESS AND THE PROPER WIPING OF YOUR DATA. Trying to "brick" your device seems like a good idea until you understand that even semi-competent adversaries have no problem retrieving data from a bricked device. Factory resetting is the most effective method of overwriting data and that's what this killswitch strives for. The factory reset process can only perform properly with essential system components performing properly. Do your research before removing certain system packages.*

*Tip: Use [ChatGPT](https://chatgpt.com/) to help you quickly format your package names into the command without having to type everything yourself.*    

- **Configure the external SD card that will be wiped (if you have one):**

You will need the ID of your external SD card in order for the command to know which SD card to wipe.

List storage drives:
```
adb shell df
```
Find your SD card in the list of drives and format its ID into the command:
```
adb shell rm -rf /storage/<SD card ID>/*
```

- **Input what your new PIN password will be:**
```
adb shell cmd lock_settings set-pin --old <your old PIN number/type in any random PIN number if you didn't originally have one> <your new PIN number (4-16 digits)>
```
Keep in mind that you will need to remember this new PIN and input it after factory resetting to regain access to your device again.

[ꜛBACK TO INDEXꜛ](#INDEX)

## TODO <a name="TODO"></a>

**This section is for contributors who'd like to help add to the killswitch. The commands I want to add are stated with potential leads regarding their syntax. Blank code blocks are due to not finding solutions online, and further testing being required.**

**Add a command to disable Wi-Fi Calling.**   
```
?: settings put global wfc_ims_enabled 0
```
**Add a command to forget saved Wi-Fi networks.**
```
?: wpa_cli remove_network <The name (SSID) of your network>; wpa_cli save_config
```
**Add a command to disable Mobile Hotspot.**
```
?:
```
**Add a command to forget paired Bluetooth devices.**
```
?: settings put secure bluetooth_offloaded_paired_devices <your Bluetooth device's MAC address>
```
**Add a command to turn on Airplane Mode**
```
?:
```
**Add a command to disable guest profiles.**
```
?:
```
**Add a command to delete Private Space.**
```
?:
```
**Add a command to disable Secure Folder.**
```
?:
```
**Add a command to format an external SD card, if present.**
```
?: mkfs.vfat /dev/block/vold/179:1
```
**Add a command to unmount an external SD card, if present.** 
```
?: umount /mnt/sdcard
```
```
?: umount /storage/sdcard 
```
**Add a command to send an emergency text message.**
```
?: am start -a android.intent.action.SENDTO -d sms:<phone number> && input text 'SOS' && input keyevent 66
```
```
?: am start -a android.intent.action.SENDTO -d sms:+<phone number>   --es  sms_body "SOS --ez exit_on_sent false
```

[ꜛBACK TO INDEXꜛ](#INDEX)

Credit to [Venice AI](https://venice.ai/) for the AI generated picture.
