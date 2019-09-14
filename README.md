# rpi-auto-backlight
A Python service for automatically adjusting the brightness of the official Raspberry Pi 7" touch display based on the signal received from an external brightness sensor.

## Requirements

## Installation

1. Create this udev rule to update permissions, otherwise you'll have to run the service as root:
```console
echo 'SUBSYSTEM=="backlight",RUN+="/bin/chmod 666 /sys/class/backlight/%k/brightness /sys/class/backlight/%k/bl_power"' | sudo tee -a /etc/udev/rules.d/backlight-permissions.rules
```
2. Verify python 3 is installed
```console
$ python3 --version
Python 3.7.3
```
3. Install required packages
```console
sudo apt-get install git
```
4. Clone the project repository
```console
git clone https://github.com/dquercus/rpi-auto-backlight.git
```
5. Install python dependencies for the project
```console
sudo apt-get install python3-pip python3-rpi.gpio
sudo pip3 install rpi-backlight
```
6. Deploy script and register service
```console
sudo cp rpi-auto-backlight-gpio.py /usr/local/bin/
sudo chmod +x /usr/local/bin/rpi-auto-backlight-gpio.py
sudo cp rpi-auto-backlight-gpio.service /etc/systemd/system/
sudo chmod 777 /etc/systemd/system/rpi-auto-backlight-gpio.service
```
7. Add user to gpio group
```console
sudo usermod -a -G gpio alarmclock
```
8. Enable service so its executed on boot
```console
sudo systemctl daemon-reload
sudo systemctl enable rpi-auto-backlight-gpio.service

```

# Configuration

The script supports the following configuration options via configuration file:

By default it will search for /etc/auto_backlight.cfg Use system variable AUTO_BRIGHTNESS_CONFIG_FILE to point to a different path. 