# rpi-auto-backlight
A Python service for automatically adjusting the brightness of the official Raspberry Pi 7" touch display based on the signal received from an external brightness sensor.

## Requirements

## Installation

**Note:** Create this udev rule to update permissions, otherwise you'll have to run the service as root:

```console
$ echo 'SUBSYSTEM=="backlight",RUN+="/bin/chmod 666 /sys/class/backlight/%k/brightness /sys/class/backlight/%k/bl_power"' | sudo tee -a /etc/udev/rules.d/backlight-permissions.rules
```
