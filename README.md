# Repetier-Firmware for Overlord Pro 3D printer
This is a fork of Repetier firmware for Overlord Pro 3D printer. It’s based on the earlier fork by [Jay](https://github.com/jayz28/Repetier-Firmware-OLP) applied to the more recent version of the upstream Repetier firmware. This is firmware is not official and may have bugs. Keep that in mind and use it on your own risk!

## How it differs from the official firmware
* Z height, end stop, towers angle correction 
* auto bed levelling (3 points or mesh leveling)
* bed distortion correction
* many printer settings are stored in EEPROM and can be changed (or backed up) without re-flashing firmware.
Also it should be noted that this firmware  official firmware has features that this firmware doesn’t have (e.g. backup on power failure, etc)

## Differences from the previous Repetier fork
- Friendlier filament reloading routine
- Menu option to turn the LED lights on/off
- many changes due to mainline Repetier version update (e.g. bed distortion correction, pre-heating, different menu structure and many more)

## How to update
You can ether compile it yourself using Arduino software (use development_olp branch) or grab a binary from the releases page (starting with version 0.92.9, older ones are from upstream repetier and are not supposed to run Overlord) and flash it with Cura via USB connection. Released binaries are made for machines with 12V heaters (earlier Kickstarter machines). Same firmware should work for machines with 24V heaters, but you'll need to adjust Extruder 1 Max PID value to 255 (default value is 160). Don't do that if you have 12V heater or you not sure which heater you have because it may damage your heater!
After flashing the firmware for the first make sure to [calibrate your Z-height](http://www.dreammaker.cc/forum/viewtopic.php?f=6&t=163) and level the bed.

### Getting started
Repetier firmware works with PC-based printer control software - [Repetier Host](https://www.repetier.com/). You can use it or other similar software(such as [Pronterface](www.pronterface.com/)). This PC-software is useful for configuration (e.g. for leveling configuration and EEPROM changes), but it's not strictly required. You can print from SD card and even adjust some EEPROM values from the LCD menu.

### Printer configuration options for Repetier Host
Here are the important ones:
- Baudrate 250000
- Dimension X=0, Y=0, Z=MAX 
- Printer Type: Delta Printer
- Diameter: 170mm
- Height: 260mm

### Slicing software
To make printable g-code file from .stl file you need a slicer. You can either keep using Cura slicer or use some [other slicer](http://slic3r.org/). If you want to keep using Cura make sure to change g-code flavor setting to RepRap. 
You can also use custom start g-code to purge residuary filament at the start of the print as the official firmware does and workaround some rare power overload issues. Here is a version for [Slic3r](https://github.com/mplatov/Repetier-Firmware/wiki/G-code-start-script-for-slic3r) and here is the one for [Cura](https://github.com/mplatov/Repetier-Firmware/wiki/G-code-start-script-for-Cura).

## Bed leveling

### Manual leveling
It can be a bit time consuming, but it gives excellent results. There is more than one way to do it. 
You can refer to [this](http://www.dreammaker.cc/forum/viewtopic.php?f=6&t=163) excellent guide on how to do it with OLP.

### Automatic bed leveling
This is one requires Z-probe hardware sensor fixed on the print head. The simplest variant of Z-probe is a microswitch mounted in such way that it shortens when print head moves down and touches it against the bed. There are [few options](https://www.thingiverse.com/search?q=z-probe+mount) of Z-probe mounts on thingiverse (or you can always make you own!) With this firmware you can connect Z-probe to the SEN header on the small PCB on top the print head (middle pin and the right pin). Once connected you can follow the [official Repetier guide](https://www.repetier.com/documentation/repetier-firmware/z-probing/) which describes how to test your Z-probe and how to leveling and distortion corrections. Make sure to set Z-Probe height offset to match your Z-probe!

## Filament Reloading 
There are two ways to do. 
### With Repetier Host software. 
With the control panel in Repetier host you heat up the nozzle, then send retraction commands to pull the filament back until you can remove it. Replace the filament and then send extrude commands to push the filament into the nozzle.

### With LCD menu option
* Select Control, Change filament. 
* Chose heat up the nozzle option (the exact temperature depends on your filament, but generally speaking it's around 210C for PLA and 230C for ABS). 
* Once nozzle is heated, selected Change filament option. The printer will then retract about 700mm of filament. You should be able to easily remove if from extruder's tube after retraction. If retraction wasn't enough you retract/extrude a bit more with up/down buttons.
* Replace the filament. Insert it into extruder's tube. Make sure that extruders pull the filament when you press Down button and select Continue option on the LCD screen.
* Extruder will push filament though the tube into the print head. Once it finish pulling adjust with Down button manually until the material start come out of the nozzle.
* Select Continue and turn off nozzle heat on the next menu.
 
## EEPROM
Many settings are stored in EEPROM by default, which means you can change them without having to recompile the firmware. It also means that if some constant value was changed and this value has corresponding EEPROM storage option, it won't get updated even if you reflash the firmware! So if you updated from previous repetier firmware and something is not working correctly (abnormally slow movements, etc) make sure to reset EERPOM to defaults by sending M502 G-code or through LCD menu. You can also back up the contents of your EEPROM first using Repetier Host, which is especially useful if you customized something to the specifics of your printer (e.g. max Z-height, tower offsets or angle corrections). 

## Repetier Documentation

For documentation on generic Repetier firmware please visit [http://www.repetier.com/documentation/repetier-firmware/](http://www.repetier.com/documentation/repetier-firmware/)


## Other Features of Repetier firmware

- Supports cartesian, delta and core xy/yz printers.
- RAMP acceleration support.
- Path planning for higher print speeds.
- Trajectory smoothing for smoother lines.
- Nozzle pressure control for improved print quality with RAMPS.
- Fast - 40000 Hz and more stepper frequency is possible with a 16 MHz AVR.
- Support for Arduino Due based boards allowing much faster speeds. 
- Multiple extruder supported (max. 6 extruder).
- Standard ASCII and improved binary (Repetier protocol) communication.
- Autodetect the command protocol, so it will work with any host software.
- Important parameters are stored in EEPROM and can easily be modified without
  recompilation of the firmware.
- Automatic bed leveling.
- Mixed extruder.
- Detection of heater/thermistor decoupling.
- 2 fans plus thermistor controlled fan.
- Multi-Language support, switchable at runtime.
- Stepper control is handled in an interrupt routine, leaving time for
  filling caches for next move.
- PID control for extruder/heated bed temperature.
- Interrupt based sending buffer (Arduino library normally waits for the
  recipient to receive written data)
- Small RAM memory print, resulting in large caches.
- Supports SD-cards.
- mm and inches can be used for G0/G1
- Arc support
- Dry run : Execute yout GCode without using the extruder. This way you can
  test for non-extruder related failures without actually printing.

## Controlling firmware

Also you can control the firmware with any reprap compatible host, you will only get
the full benefits with the following products, which have special code for this
firmware:

* [Repetier-Host for Windows/Linux](http://www.repetier.com/download/)
* [Repetier-Host for Mac](http://www.repetier.com/download/)
* [Repetier-Server](http://www.repetier.com/repetier-server-download/)

