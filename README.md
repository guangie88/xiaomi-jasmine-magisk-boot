# xiaomi-jasmine-magisk-boot

Contains Magisk v16.0 patched `boot.img` for Xiaomi Mi A2 (Jasmine), from build
number `OPM1.171019.011.V9.6.5.0.ODIMIFE`. All boot images are located within
the build number directory.

The original boot image is named `boot.img`, and the Magisk patched boot image
is named `patch_boot.img`.

## How to flash the patched `boot.img` to get Magisk (and root)

If you are already familiar with `fastboot` and the commands in general, simply
go to [Fastboot flash](#fastboot-flash) to check out the flashing commands.

You will need `fastboot` command to perform the flash.

### Restart into `fastboot` via button presses

Simply reboot the phone. Hold down the Volume Down button just at the instance
when the phone turns dark to start the boot sequence. The phone should instead
boot into `fastboot` mode.

### Restart into `fastboot` via ADB

You will need the `adb` command.

Below assumes Linux `bash` environment with both commands located in `PATH`, and
the phone should have already been connected to the machine.

If the phone is already booted into Android, you can go into:

`Settings > System > About phone`

and scroll down all the way, and keep tapping on Build number, to enable
`Developer options`.

Now go back one screen and enter into `Developer options`, and enable
`Android debugging` to allow the machine to send commands to the phone via
`adb`.

Once done, go to the phone and accept the host key. Then go back to the machine
and in `bash` run

```bash
adb devices
```

to make sure that the phone is being correctly detected.

To reboot into `fastboot` mode, run

```bash
adb reboot bootloader
```

### Fastboot flash

In `fastboot` mode, run

```bash
fastboot devices
```

and make sure the phone is being correctly detected.

To flash the patched boot image to install Magisk, run

```bash
fastboot flash boot_a patched_boot.img
fastboot flash boot_b patched_boot.img
```

Note that this is a destructive action and may cause bootloop if the build
number of your current build is different, so make sure that the build numbers
on the phone and the directory name match.

Reboot the phone with the following command

```bash
fastboot reboot
```

The phone should hopefully reboot back into the Android OS successfully.

Now you should have the Magisk v16.0 installed, which means your phone can now
perform `root` operations.

## Acknowledgement

The `payload.bin` file was extract from the OTA dump zip file in
<https://forum.xda-developers.com/mi-a2/how-to/ota-v9-6-5-0-odimife-t3823445>.

The original `boot.img` is then extracted from the above `payload.bin` using
[`extract_android_ota_payload`](https://github.com/cyxx/extract_android_ota_payload).
