# Game Boy Zero

> Looking for a no frills multi system emulator that fits in your pocket? Look no further! This is it, Gameboy Zero - Handheld Edition!!!
>
> This project is a collaboration between  [moosepr](https://github.com/moosepr/) and  [32teeth](https://github.com/32teeth)
>
> We are **live** on [KickStarter](https://www.kickstarter.com/projects/sparky/gameboy-zero-handheld-edition)
>
> **Thank you everyone for your support.**





<img src="images/gameboyzero-logo.png" width="75%">



<img src="images/kickstarter-logo.png" width="25%">







## Introduction

### Gameboy Zero - Handheld Edition
Raspberry Pi Zero based arcade in you pocket



### Features:

* 2.8" TFT LCD
* Gameboy button layout interface
* Raspberry Pi Zero
* Battery Charger
* Shoulder / Trigger buttons
* 60 frames per second display



### Platforms:

The boards have been tested with the following platforms:

- [x] [Emulation Station](http://www.emulationstation.org/)
- [ ] [Lakka](http://www.lakka.tv/)
- [x] [RetroPie](https://retropie.org.uk/)

Other platforms are yet to be tested.





#### Eagle CAD Library

With our project we have included a few custom Eagle CAD Libraries

* [Displays](/library/GameboyZeroHandheldDisplays.lbr)
* [Raspberry Pi Zero](/library/RPI-Zero.lbr)


#### 3D Render
You can play with the [3D Render Here](https://32teeth.github.io/Gameboy-Zero-Kickstarter/)




## Details

| Version               | Top                                      | Bottom                                   |
| --------------------- | ---------------------------------------- | ---------------------------------------- |
| Flat Screen           | <img src="silks/gameboyzero-top.bmp" height="350"> | <img src="silks/gameboyzero-bottom.bmp" height="350"> |
| Composite Board       | <img src="images/eagleUp_gameboyzero_board_top.png" height="350"> | <img src="images/eagleUp_gameboyzero_board_bottom.png" height="350"> |
| Holiday Express Board | <img src="images/eagleUp_gameboyzero-holiday_board_top.png" height="350"> | <img src="images/eagleUp_gameboyzero-holiday_board_bottom.png" height="350"> |



## Custom Silk Screen

if you plan on changin the silk design,
* use the [gameboyzero sketch](design/gameboyzero) file
* all fonts required are in the  [fonts folder](fonts/)
* export artboard (with either 'TOP' or 'BOTTOM' group active as JPG)

*converting jpg to 2 color bitmap*
```shell
# imagemagick is required brew package
$ brew install imagemagick

# convert (replace with your file names and outputs)
# ex: convert "gameboyzero-top.jpg" -depth 1 -monochrome -colors 2 "gameboyzero-top.bmp"
$ convert "[jpg to convert]" -depth 1 -monochrome -colors 2 "[bmp to output]"
```



#### Import bmp in Eagle CAD.

* File > Import > Bitmap
* Select your bitmap
* Select **only** black color
* Change scale type to *mm*
* Change scale to ***0.086** (disregard the scale in the images)*
* Place it on layer layer 21 (tPlace) or 22 (bPlace)



| Top                                     | Bottom                                   |
| --------------------------------------- | ---------------------------------------- |
| <img src="images/eagle-import-top.png"> | <img src="images/eagle-import-bottom.png"> |



## Plexi Case

The design for the layered plexi case are in the [case folder](/case)

*It consists of 5 layers:*
* TOP (optional for screen protection)
* SCREEN (fits over 2.8" TFT LCD)
* PI ZERO (fits around the Raspberry Pi Zero and TF4056 Module)
* BOTTOM (fits on top of the Raspberry Pi Zero)
* BATTERY (this is the layer to hold the LiPo battery in place)



<img src="case/gameboyzero-case.png">





## Custom Config

> If you do not plan on using the provided Raspberry Pi image, simply [download](https://retropie.org.uk/download/) the latest RetroPie pre made image for the Raspberry Pi Zero

Once you have installed RetroPie onto your SD card and mounted it on your Pi follow along with the [Installation Instructions](INSTALL.md) document provided
