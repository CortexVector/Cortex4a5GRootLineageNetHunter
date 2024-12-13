# IF ANYONE SEES THIS, IT IS CURRENTLY A WIP AND NOT DONE YET

# Cortex4a5GRootLineageNetHunter
This is a repo for setting up a Google Pixel 4a 5G with Lineage OS and Rooted Kali NetHunter in 2024/2025
(Written 12/08/2024 and not sure if/how much I will keep this up-to-date)

It was such a headache to do this, going down roads with broken download links, posts marked deprecated, building NetHunter images, and sites more concerned with shoving ads of their shitware down your throat than giving you what you need, so I agreggated it all here.

The main resource I found and used is Alynx's: https://xdaforums.com/t/kernel-nethunter-for-pixel-4a-5g-android-12l.4475363/
but that guide needs some modifications due to Lineage and lack of TWRP for this phone (don't want to go down the path of building out TWRP if I can help it).

Anyways, shoutout to them.

It had been years since I have done native Android dev when it was back in the Java rather than Kotlin days.
So, pardon if I mispeak on the technicals here and there.

Also, I am not responsible for fucking up your phone, obviously. Or anything you do with it.

If you run the commands from this repo's directory, they will be as written.

BTC Tip Jar: 
Other tip jar method

## Files/Apps Used
I am not going over installing ADB or Platform tools so find a guide for that.
Not in repo:
- OpenMTP for file transfer is on MacOS: https://openmtp.ganeshrvel.com/
- Payload dumper for extracting boot, dtbo, etc...: https://github.com/vm03/payload_dumper
- microG F-Droid: https://microg.org/download.html

In repo:
- Lineage OS 19.1 for bramble: https://wiki.lineageos.org/devices/bramble/
- 

## Directions
Follow the instructions on the Lineage link above for all the warnings and gotchas. Here are the simplified instructions if you know what you are doing.

### Unlock Bootloader and install OS
- Enable Dev Mode
- Enable USB Debugging
- Enable OEM Unlocking
- Trust Computer
- `adb -d reboot bootloader` to reboot to fastboot mode
- `fastboot flashing unlock` to unlock bootloader. `WARNING`: This resets phone.
- Reboot, enable dev mode, etc..
- `adb -d reboot bootloader`
- `fastboot flash boot boot.img`
- `fastboot flash dtbo dtbo.img`
- `fastboot flash --disable-verity --disable-verification vbmeta vbmeta.img` <- extra step not in Lineage instructions
- `fastboot flash vendor_boot vendor_boot.img`
- Use volume to navigate to recovery option and select it
- Tap `Factory Reset` then `Format data / factory reset` and confirm. `WARNING`: This wipes the phone (obviously)
- Go back to main menu, tap `Apply Update` then `Apply from ADB`
- Sideload Lineage OS: `adb -d sideload lineage-19.1-20221225-nightly-bramble-signed.zip`
- Reboot, enable dev mode, etc..

### Root Phone with Magisk
- Copy the `boot.img` from this repo (or extracted from Lineage image with Payload Dumper) to phone's Download dir with OpenMTP (or just nativel with Windows)
- Download and install Magisk apk: https://github.com/topjohnwu/Magisk/releases/tag/v28.1
- Open Magisk, go to "Install" > "Select and Patch a File," and select the boot image
- Copy the patched file it said it wrote back to computer
    Look for the output file name and replace the file in this command with whatever yours is:
    `adb pull /sdcard/Download/magisk_patched-28100_kcPD9.img`
- `adb reboot bootloader`
- `fastboot flash boot magisk_patched-28100_kcPD9.img`
- Reboot and launch Magisk app (you will see a stub Magisk app if you have wiped your data; use it to bootstrap to a complete Magisk app), and you will see a prompt asking for environment fix; click and wait for the reboot

### Install Busybox via F-Droid
- Install F-Droid from: https://f-droid.org/en/
- Search for "Busybox" and install the one-click installer one
- Open it and grant superuser permissions
- Click to install it
- Reboot

### Install Alynx Kernel
- Install Franco Kernel Manager and install it: https://apkmody.com/apps/franco-kernel-manager
- Copy `Alynx-4A-5G-Build.zip` to phone's Download dir with OpenMTP (or just nativel with Windows)
- Copy `Wireless_firmware.zip` to phone's Download dir with OpenMTP (or just nativel with Windows)
- Open Franco Kernel Manager, click hamburger, then Flasher, then Manual Flasher, then select `Alynx-4A-5G-Build.zip` and flash/reboot.
- Open the Magisk app.
- Go to "Modules" > "Install from Storage."
- Select `Wireless_firmware.zip and install it.
- Hit Reboot.

### Install NetHunter
- Install Kali NetHunter App Store from: https://store.nethunter.com/
- From it install NetHunter Terminal, open it, and grant superuser permissions
- From NetHunter App Store install the full pack at the top, grant superuser permissions, and other permissions, etc.. etc..
- Copy `nethunter-20241205_010950-bramble-twelve-rooted-kalifs_full.zip` to phone's Download dir with OpenMTP (or just nativel with Windows)
- Go to Kali Chroot manager in it, click install, make sure full is selected
- For the full path, use `/storage/emulated/0/Download/kali-nethunter-rootfs-full-arm64.tar.xz`

