# Arduino Package

This repo provides developers with the ability to load TelosAir boards into the Arduino IDE.

## Package Path

https://raw.githubusercontent.com/Potsdam-Sensors/ArduinoPackage/refs/heads/main/package_telosair_index.json

Paste this path in Arduino IDE via:

```File->Preferences->Additional Board Manager URLS```


## Adding a new Duet Version
### Update ```boards.txt```
1. Navigate to ```ArduinoPackage/telosairBoards/telosairDuets/boards.txt```
2. Copy and paste any variant/mark block. For Example:
```
d_mk1.name=TelosAir Duet Mk1
d_mk1.vid.0=0x1B4F
d_mk1.pid.0=0x214F
d_mk1.vid.1=0x1B4F
d_mk1.pid.1=0x215F
d_mk1.upload.tool=bossac
d_mk1.upload.protocol=sam-ba
d_mk1.upload.maximum_size=262144
d_mk1.upload.use_1200bps_touch=true
d_mk1.upload.wait_for_upload_port=true
d_mk1.upload.native_usb=true
d_mk1.build.mcu=cortex-m0plus
d_mk1.build.f_cpu=48000000L
d_mk1.build.usb_product="Duet Mk1"
d_mk1.build.usb_manufacturer="TelosAir"
d_mk1.build.board=SAMD_ZERO
d_mk1.build.core=arduino
d_mk1.build.extra_flags=-D__SAMD21G18A__ {build.usb_flags}
d_mk1.build.ldscript=linker_scripts/gcc/flash_with_bootloader.ld
d_mk1.build.openocdscript=openocd_scripts/arduino_zero.cfg
d_mk1.build.variant=mk1
d_mk1.build.variant_system_lib=
d_mk1.build.extra_combine_flags=
d_mk1.build.vid=0x1B4F
d_mk1.build.pid=0x214F
d_mk1.bootloader.tool=openocd
d_mk1.bootloader.file=zero/telosair_duet_mk1.bin
```
3. Next update the follwing bolded areas with the new variant name. I've provided an example for a hypotheical mk5 with some additional comments/observations in italics.

**d_mk5**.name=**TelosAir Duet Mk5** <U>*The name that shows up in Arduino IDE*</u><br>
<u>*I don't know what the id's are for, but duplicate id's don't seem to matter*</u><br>
**d_mk5**.vid.0=0x1B4F<br> 
**d_mk5**.pid.0=0x214F<br>
**d_mk5**.vid.1=0x1B4F<br>
**d_mk5**.pid.1=0x215F<br>
**d_mk5**.upload.tool=bossac<br>
**d_mk5**.upload.protocol=sam-ba<br>
**d_mk5**.upload.maximum_size=262144<br>
**d_mk5**.upload.use_1200bps_touch=true<br>
**d_mk5**.upload.wait_for_upload_port=true<br>
**d_mk5**.upload.native_usb=true<br>
**d_mk5**.build.mcu=cortex-m0plus<br>
**d_mk5**.build.f_cpu=48000000L<br>
**d_mk5**.build.usb_product="Duet **Mk5**"<br>
**d_mk5**.build.usb_manufacturer="TelosAir"<br>
**d_mk5**.build.board=SAMD_ZERO<br>
**d_mk5**.build.core=arduino<br>
**d_mk5**.build.extra_flags=-D__SAMD21G18A__ {build.usb_flags}<br>
**d_mk5**.build.ldscript=linker_scripts/gcc/flash_with_bootloader.ld<br>
**d_mk5**.build.openocdscript=openocd_scripts/arduino_zero.cfg<br>
**d_mk5**.build.variant=**mk5** ***<u>*important***, corresponds with the variants folder*</u> <br>
**d_mk5**.build.variant_system_lib=<br>
**d_mk5**.build.extra_combine_flags=<br>
**d_mk5**.build.vid=0x1B4F<br>
**d_mk5**.build.pid=0x214F<br>
**d_mk5**.bootloader.tool=openocd<br>
**d_mk5**.bootloader.file=zero/telosair_duet_**mk5**.bin <u>*This a copy of the sparkfun bootloader and, I belive hasn't actually chanage between duet marks.*</u><br>

### Update Bootloader (Optional)
1. Navigate to ```./ArduinoPackage/telosairBoards/telosairDuets/bootloaders/zero```
2. Copy a previous .bin and .hex file, or provide your own.
3. Make sure this change is reflected in ```boards.txt``` line ```d_mk1.bootloader.file=zero/telosair_duet_mk1.bin```

### Place Variants
1. Navigate to ```./ArduinoPackage/telosairBoards/telosairDuets/variants```
2. Copy a previous ```Mk``` folder. 
3. Rename the copied folder so that matches the name of the line ``` d_mk?.build.variant=mk? ``` in ```boards.txt```.
4. Upload the new ```variant.h``` and ```variant.cpp``` files.

### Create a new package zip file
1. Navigate to ```/ArduinoPackage/telosairBoards```
2. Create a .zip of ```telosDuets```
3. Move the new ```telosDuets.zip``` into ```/ArduinoPackage``` replacing the old .zip

### Grab zip information (Checksum and Size)
1. Navigate to ```/ArduinoPackage```
2. Utilizing ``ls -al`` grab the new file size of ```telosDuets.zip```.
3. Save this size for the next step.
4. Utilizing ```sha256sum /ArduinoPackage/telosDuets.zip``` grab the new checksum of the .zip file.
5. Save this checksum for the next step.


### Update ```package_telosair_index.json```
Note: This filename must be in the following format ```package_name_index.json```.

1. Navigate to ```./ArduinoPackage/package_telosair_index.json```
2. Place the newly recorded checksum and size in their respective .json entries.<br> ``` "checksum": "SHA-256:bfe2213135c682e63638419b189e196c44873fdf5f83288f88b81e3bca1975c3",```<br>
          ```"size": "1333767",```
3. Add the new board varient to the boards arrays. <br>```          "boards": [
            { "name": "TelosAir Duet Mk4"},{ "name": "TelosAir Duet Mk3"},{ "name": "TelosAir Duet Mk1"},{ "name": "TelosAir Duet Mk5"}
          ]```
4. Increment the version. This is the verison of the board manager seen by Arduino IDE<br>
```"version": "1.0.13",```

## Reference Articles

- [Adding custom Zero-based boards to the Arduino IDE](https://forum.arduino.cc/t/adding-custom-zero-based-boards-to-the-arduino-ide/394499)
- [Arduino CLI: `package_index.json` Specification](https://arduino.github.io/arduino-cli/1.2/package_index_json-specification/)
- [Arduino CLI: Platform Specification](https://arduino.github.io/arduino-cli/1.2/platform-specification/)
