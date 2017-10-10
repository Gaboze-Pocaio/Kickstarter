# Gameboy Zero Installation

> This document is intended for a somewhat advanced user base, but we have made it simple enough to follow along for anyone


### Prerequisites
1. Fresh RetroPie install mounted on an SD Card
2. Raspberry Pi Zero *(preferably WiFi edition)*
3. Keyboard
4. OTG USB Cable (to plug in a USB A type keyboard)
5. Mini HDMI (you know, to see things on a screen)

## Step 1
> Setup Raspberry Pi requirements

With your Keyboard and HDMI cable plugged in, go ahead and turn on your Raspberry Pi Zero
Allow it to go through the boot requirements (RetroPie will reboot once on it's own)

Once the Raspberry Pi boots into RetroPie, it will start EmulationStation automatically.

You will see a screen telling you to either set up a controller or press F4 to exit.

* Press F4 to exit to terminal
* Run the Rapsberry Pi configuration program
```shell
sudo raspi-config
```
In the Interfaceing Options menu, you will need to 
1. Enable SSH
2. Enable SPI

In the Advance Options menu, you will need to
1. Exapnd Filesystem
2. Disable Overscan

Once these are complete, you will be asked to reboot you Raspbery Pi.
Do so.

### WiFi

Once the Raspberry Pi reboots into EmulationStation, use your keyboard and navigate to RetroPie configuration

Select WiFi and enter your SSID/PSK information as necessary

Once you have connected to your WiFi network, you can go back to your laptop and continue via SSH from there

#### Optional

Should you choose to work completely from terminal, hit F4 from the EmulationStation menu to get to terminal
, 
```shell
sudo iwlist wlan0 scan
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```

```shell
network={
  ssid="your ssid"
  psk="your password"
}
```

```shell
sudo systemctl daemon-reload
ifconfig wlan0
```

If you have done your WiFi correctly from here you can use SSH

```
ssh pi@[ip address]
```

## Step 2
> We will now be connecting the 2.8" ILI9341 TFT LCD module, so you can set your other keyboard aside for now

From you computer *(not the Raspberry Pi)*

### TFT Device

Enter the following to add a Loadable Kernel Module for the TFT Device
```shell
sudo modprobe fbtft_device custom name=fb_ili9341 gpios=reset:25,dc:24,led:18 speed=80000000 fps=60 bgr=1 brightness=100 rotate=90
```

Confirm the module has been loaded
```shell
dmesg | tail 
```

See the TFT LCD module details
```shell
fbset -fb /dev/fb1
```

Lets test that we can show 'stuff' on the TFT LCD module (this will simply add static noise)
```shell
cat /dev/urandom > /dev/fb1
```

Map the console to the TFT LCD Module
*you should now see a scaled terminal window on the TFT*
```shell
con2fbmap 1 1
```

Should you ever need to stop the mapping, reverse the command
```shell
# switch tft to hdmi
con2fbmap 1 0
```


## Step 3
### Automate the above on startup

Open the modules file
```shell
sudo nano /etc/modules
```

Add these lines to the bottom
```shell
spi-bcm2835
fbtft_device
```

*Hit 'CTRL+X' and 'Y' to confirm the save*

### Create a configuration file for the TFT LCD Module

This will create an empty file named fbtft.conf
```shell
sudo nano /etc/modprobe.d/fbtft.conf
```

Add this line to the empty file
```shell
options fbtft_device custom name=fb_ili9341 gpios=reset:25,dc:24,led:18 speed=80000000 fps=60 bgr=1 brightness=100 rotate=90 custom=1
```

*Hit 'CTRL+X' and 'Y' to confirm the save*

### Installing the Framebuffer Copy tool

The following lines will download and install the Frambuffer Copy tool in a folder in the root directory

```shell
cd
sudo apt-get install cmake
git clone https://github.com/tasanakorn/rpi-fbcp fbcp
cd fbcp/
mkdir build
cd build/
cmake ..
make
sudo install fbcp /usr/local/bin/fbcp
fbcp
```

*Hit CTRL+C to exit 'fbcp'*

### Add Framebuffer Copy to our boot sequence
In order to have a command or program run when the Pi boots we will edit the 'rc.local' file

```shell
sudo nano /etc/rc.local
```

Add the following **just before** the ```exit 0``` line
```shell
fbcp&
```

*Hit 'CTRL+X' and 'Y' to confirm the save*

Your file *(if you have no other commands running)* will look something like this
```shell
### replace with this
_IP=$(hostname -I) || true
if [ "$_IP" ]; then
  printf "My IP address is %s\n" "$_IP"
fi

fbcp&

exit 0
```

## Step 4
> Now we are ready to configure the GPIO on the Raspberry Pi as a controller

### Install RetroGame

```shell
cd
curl -O https://raw.githubusercontent.com/adafruit/Raspberry-Pi-Installer-Scripts/master/retrogame.sh
sudo bash retrogame.sh
```
When you run the retrogame.sh file, you will be presented with a menu.

Select the **PiGRRL 2 Controls** option, and the program will continue to install some modules.

Once complete, you will be asked if you want to reboot. 
#### Don't reboot just yet

## Step 5
> You will now be assigning keyboard inputs to action buttons of the controller (GPIO pins on the Raspberry Pi)

### RetroGame Configuration

Edit the RetroGame configuration file
```shell
sudo nano /boot/retrogame.cfg
```

You can replace the entire file with the following
* **pro tip**:Hit CTRL+K on you keyboard to delete lines*

```shell
# Here's a pin configuration for the PiGRRL 2 project:
LEFT      21  # Joypad left
RIGHT     20  # Joypad right
UP         7  # Joypad up
DOWN      12  # Joypad down
LEFTCTRL  19  # 'A' button
LEFTALT    5  # 'B' button
Z          6  # 'X' button
X         13  # 'Y' button
SPACE     16  # 'Select' button
ENTER     26  # 'Start' button
A         18  # Left shoulder button
S         27  # Right shoulder button
Q         15  # Left trigger button
W         17  # Right trigger button
ESC       16 26  # Exit ROM; PiTFT Button 1
```

*Hit 'CTRL+X' and 'Y' to confirm the save*

### EmulationStation Input Configuration
> This is really just a visual check to ensure your file looks as below

Open the EmulationStation Input configuration file
```shell
sudo nano ~/.emulationstation/es_input.cfg
```

Ensure that your file's content are **exactly** as below
```xml
<?xml version="1.0"?>
<inputList>
  <inputAction type="onfinish">
    <command>/opt/retropie/supplementary/emulationstation/scripts/inputconfiguration.sh</command>
  </inputAction>
  <inputConfig type="keyboard" deviceName="Keyboard" deviceGUID="-1">
    <input name="pageup" type="key" id="97" value="1"/>
    <input name="start" type="key" id="13" value="1"/>
    <input name="up" type="key" id="1073741906" value="1"/>
    <input name="a" type="key" id="1073742048" value="1"/>
    <input name="b" type="key" id="1073742050" value="1"/>
    <input name="down" type="key" id="1073741905" value="1"/>
    <input name="pagedown" type="key" id="115" value="1"/>
    <input name="right" type="key" id="1073741903" value="1"/>
    <input name="x" type="key" id="122" value="1"/>
    <input name="select" type="key" id="32" value="1"/>
    <input name="y" type="key" id="120" value="1"/>
    <input name="left" type="key" id="1073741904" value="1"/>
    <input name="a" type="key" id="1073742048" value="1"/>
    <input name="b" type="key" id="1073742050" value="1"/>    
  </inputConfig>
</inputList>
```

If all is good as above, hit CTRL+X to exit the editor and

# Step 6
> The final reboot, you can now unplug the HDMI from the Raspberry Pi

Reboot
```shell
sudo reboot now
```

RetroPie will reboot and start EmulationStation *(all on the tiny screen)*

Once EmulationStation is up, you will again see the 'No Device Configured' screen...  ...***Don't Panic***

With your keyboard plugged in, press and hold 'Enter' until the Controller Configuration shows up on the screen

Hit your keys on your keyboard as mapped above, skip the analog sticks
* **pro tip**:When you get to the section*

# Step 7
> This one is all on you

We **DO NOT** provide any system or game *'roms'*, for that you will need Google.


