# OpenCore Hackintosh EFI Files for Thinkpad T450s
These are OpenCore 1.0.5 EFI files to get Mac OS running on a Thinkpad T450s. A regular Thinkpad T450 may work as well, but I do not have a machine to test.
Keep in mind these are the bare minimum to install and have a somewhat using system. I have not done extensive testing and not everything may work. **Yes, it will be a bit sluggish. This is old hardware.**

<img width="1600" height="900" alt="t450s-monterey" src="https://github.com/user-attachments/assets/31dc1adf-294b-412e-b370-bb32306427ec" />

## Requirements:
 - A functioning frontal lobe
 - A basic understanding of OpenCore and Hackintoshing
 - The following BIOS settings set:
    - `Security Chip`: **Disabled**
    - `Memory Protection -> Execution Prevention`: **Enabled**
    - `Intel Virtualization Technology`: **Enabled**
    - `Virtualization -> VT-Directed IO`: **Disabled**
    - `Bottom Cover Tamper Detection`: MUST BE **Disabled**
    - `Computrace`: **Disabled**
    - `Secure Boot`: **Disabled**
    - `UEFI/Legacy Boot`: **UEFI Only**
    - `Fingerprint Sensor`: **Disabled** (It prevents waking up from sleep for whatever reason.)
    - `CSM Support`: **Yes**

## Tested Hardware
 - CPU: `i5-5300U @ 2.60 GHz`
 - GPU: `Intel HD Graphics 5500`
 - LAN: `Intel I218-LM Ethernet Controller`
 - WAN: `Intel Wireless 7265`
 - Dual Batteries

## Supported MacOS Versions
These are MacOS versions that I have gotten to boot and to be usable, but that does not mean EVERYTHING works.
 - MacOS Monterey (12.7.4 and 12.7.6 tested)

## What works and what doesn't?
### What works
 - Sleep (I cannot test waking because my machine has a BIOS password, so I cannot disable the problematic fingerprint sensor)
 - Intel Wifi
 - Ethernet
 - Built-in speakers & Microphone
 - Dual Batteries
 - Touchpad & Trackpoint
 - USB 2.0 / 3.0
 - Camera
 - FileVault (disk encryption)
 - OS Keyboard Backlight Control - see Post Install section to add the System Preference Pane
 - Thinkpad Light Customization - see Post Install section to add the System Preference Pane
 - Battery Charging Limits (by battery included) - see Post Install section to add the System Preferences Pane

### What doesn't
 - VGA
 - SD Card Reader
 - Anything EC Related, including fan control (Need assistance with this!)

### What I haven't tested
 - WWAN / 4G Modem
 - Mini DP
 - Apple Services (iMessage, Facetime, App Store, AirDrop, Continuity, etc.)
 - Broadcom network adapters (Should work, may need to disable Airport Kext)
 - Bluetooth (probably works)
 - DRM
 - Lots of other stuff

## Getting started
(todo!! seek out other resources for getting started, and then just copy the EFI folder once you have a MacOS image on your USB stick)

## Post-Install
Once you're up and running, you can follow these steps to get a better experience! I recommend editing your Config.plist file with [OCAT](https://github.com/ic005k/OCAuxiliaryTools), which provides an easy to use GUI editor.

### 1. Disable Verbose mode (scrolling text on startup), and other debugging stuff such as AppleDebug and ApplePanic
 - Open up Config.plist in your favorite plist editor
 - Go to `NVRAM` -> `Add` -> `7C436110-AB2A-4BBB-A880-FE41995C9F82`
 - Edit the `boot-args` option, and delete the `-v` argument. This disables verbose mode.
 - To disable AppleDebug and ApplePanic, go to `Misc` -> `Debug`
 - Set `AppleDebug` and `ApplePanic` to `false`, or if you're using OCAT just uncheck them.
 - Save the file.

### 2. Disable the OpenCore OS Picker on startup (boot straight into MacOS)
 - Open up Config.plist in your favorite editor
 - Go to `Misc` -> `Boot`
 - Set `ShowPicker` to false (or uncheck it if using OCAT)
 - Save the file.

### 3. Adding a ThinkPad / YogaSMC category in System Preferences
 - Download `org.zhen.YogaSMC.plist` from this repo
 - Open up Finder on your Hackintosh, and press CMD + Shift + G to get an address bar in the middle of the window
 - Type `~/Library/Preferences` and hit Enter
 - Move that downloaded file into that folder
 - You should now have a YogaSMCPane category at the bottom of System Preferences.
 - Additionally, you can get [YogaSMCNC](https://github.com/zhen-zen/YogaSMC/releases/latest) (Get YogaSMC-**App**-Release.**dmg**) to get statistics/fan control in one click in your top bar, as well as on-screen popups for when you change Keyboard Backlight, mute your microphone, toggle caps lock, etc. I recommend it!

### 4. Getting rid of the USB Stick and moving the EFI to your computer's storage
**ONLY DO THIS AFTER YOU ARE SURE EVERYTHING WORKS!!** Save yourself a headache and troubleshoot off a USB stick.

 - Download [MountEFI](https://github.com/corpnewt/MountEFI) and follow it's installation instructions
 - Run MountEFI, and press "b" then Enter to mount the boot drive's EFI partition.
 - **If it says "There is no EFI partition associated with [disk]", follow these steps:**
   - (to write! use gparted or something to create a new 150ish MB partition called "EFI" and formatted as "FAT32")
   - Restart this section to mount the partition.
 - It will mount your EFI partition as a volume you can access from Finder.
 - Make sure you have Verbose mode, all of the debugging stuff, and ShowPicker is disabled (not required but *highly* recommended to make your experience booting up smoother)
 - Simply drag and drop the "EFI" folder (NOT it's contents, the folder itself.) from your USB stick to that mounted EFI partition.
 - Eject your USB stick, remove it, and reboot! If it boots into MacOS without your USB stick, you're all done!