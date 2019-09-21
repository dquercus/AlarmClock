# rpi-auto-backlight
A Python service for automatically adjusting the brightness of the official Raspberry Pi 7" touch display based on the signal received from an external brightness sensor.

## Requirements
- Raspberry PI
- LM393 light sensor ( [I use this one](https://www.amazon.de/gp/product/B07DP1YM5X/ref=ppx_yo_dt_b_asin_title_o05_s01?ie=UTF8&psc=1) )
- Python3
  
## Installation
1. Clone the project repository
```console
git clone https://github.com/dquercus/rpi-auto-backlight.git
```
2. Install python dependencies for the project
```console
sudo apt-get install python3-pip python3-rpi.gpio
sudo pip3 install rpi-backlight
```
3. Create this udev rule to update permissions, otherwise you'll have to run the service as root:
```console
echo 'SUBSYSTEM=="backlight",RUN+="/bin/chmod 666 /sys/class/backlight/%k/brightness /sys/class/backlight/%k/bl_power"' | sudo tee -a /etc/udev/rules.d/backlight-permissions.rules
```
4. Create service user with access to the GPIO ports
```console
adduser --system --no-create-home autobacklight
sudo usermod -a -G gpio autobacklight
```
5. Deploy script and register service
```console
cd rpi-auto-backlight
sudo cp rpi-auto-backlight-gpio.py /usr/local/bin/
sudo chmod +x /usr/local/bin/rpi-auto-backlight-gpio.py
sudo cp rpi-auto-backlight-gpio.service /etc/systemd/system/
sudo chmod 777 /etc/systemd/system/rpi-auto-backlight-gpio.service
```
6. Adjust and then copy configuration file /etc/auto_backlight.cfg
```console
cd rpi-auto-backlight
sudo cp auto_backlight.cfg /etc/auto_backlight.cfg 
```
7. Enable service so its executed on boot
```console
sudo systemctl daemon-reload
sudo systemctl enable rpi-auto-backlight-gpio.service
```

# Configuration

The script supports the following configuration options via configuration file:

| Variable |   Description      |  Default value |
|----------|:-------------:|------:|
| RPI_PORT |  GPIO port for light digital signal | 7 |
| DARK_BRIGHTNESS |  Brightness level when no light detected | 5 |
| LIGHT_BRIGHTNESS |  Brightness level when light detected | 90 |
| LOOP_PAUSE |  Indicates how ofter the system checks for a new value in the GPIO port | 2 |

Configuration file can by changed using system variable AUTO_BRIGHTNESS_CONFIG_FILE to point to a different path. 

# Acknowledgements
Thanks to [torfsen/python-systemd-tutorial](https://github.com/torfsen/python-systemd-tutorial) for the instructions for running a python script as daemon using systemd.