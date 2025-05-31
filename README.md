WIP


# Neptune2SKlipper
Elegoo Neptune 2S Wireless Mod (Klipper + Mainsail + Pi Zero 2W)

Turn your Elegoo Neptune 2S into a modern, wireless, Klipper-powered machine using a Raspberry Pi Zero 2W and UART communication.

## What you need:

- Elegoo Neptune 2S (My Neptune 2S uses a ZNP Robin Nano V1.3)
- Raspberry Pi Zero 2W
- 2x Micro SD Card (8Gb+ recommended) (Not sure if class makes a difference here, The SDCard I used is Class 1 and it works fine.)
- 5V power source
- UART connection
- MainsailOS
- KIAUH
- Soldering gear (through hole soldering)
- 4 1' lengths of 18ga wire (I used Red, Blue, Black and White)
- JST connectors (recommended)
- SSH Client (PuTTY)
- WinSCP


# Software setup (MainsailOS + Klipper)

Before you begin the process of opening your printer and poking around it's time to flash the OS on the Raspberry Pi.

## 1. Flash the OS.
Download the Raspberry Pi Imager.



Choose Device > Raspberry Pi Zero 2W

Choose OS > Other Specific-purpse OS > 3D Printing > MainsailOS

Select Storage device (1 of the 8Gb SD cards)

### Edit settings

GENERAL:
Set host name to whatever you like. (Elegoo.local)
Set username and password for SSH ( I used Pi for Username)
Configure LAN to the same network your Slicer is on. Wireless LAN country set to whatever country you are in.
Set locale

SERVICES:
Enable SSH

Save
Yes
Yes

When the OS has been flashed to the SD Card, insert into RPi and connect to USB power.

SSH into Pi using PuTTY using either the domain name you have given it or finding the IP (Find that using Client list in your Router)

Login using the credentials you made during the OS flashing.

## Printer Firmware

When you login to the Pi via SSH you should be in the root path

```
cd klipper
```

```
make menuconfig

```

### Klipper Firmware Configuration

For ZNP Robin Nano V1.3

Enable extra low-level configuration options
Micro-controller Architecture > STMicroelectronics STM32
Processor Model > STM32F407
32KiB Bootloader
Communication Interface > Serial (on USART1 PA10/PA9) ((This will use the UART header found at the bottom right of the board.))
250000 Baud Rate
Leave everything else as is.

Hit Q to save and create the config file

-----------------------------------------------------------------

When it has completed making the config file type

```
make
```

It will start compiling the config to create the new firmware .bin file

```
cp out/klipper.bin out/elegoo.bin
```

-----------------------------------------------------------------

open WinSCP



New Site > Session

Hostname > elegoo.local or IP Address to RPi

and same credentials as earlier

Enter

------------------------------------------------------------------

Insert SDCard #2 in PC

In WinSCP there will be a split window.

The left is your computer's directory and the right is the Pi's directory

On the left find the dropdown menu and find your SD card

On the right, navigate to 

```
/home/pi/klipper/out
```

Drag and drop the elegoo.bin file to your SDcard
Eject SDcard safely.

### Making the Printer and Pi as one

Power off printer and remove power lead.

Remove any spool you may have on the printer and tip it on its side.

Remove the bottom panel.

Undo the 4 screws holding the main board in place.
Remove the touchscreen (unfortunately it won't work anymore but ***it still needs to be removed to successfully flash elegoo.bin***)

Find the UART header (it will be next to the SD card slot between the black and green headers. 4 through holes)

It doesn't matter what color you use for what but I used:

- 5V - Red wire
- GND - Black wire
- Tx - Blue wire
- Rx - White wire

Solder the wires in place on the main board.

Take RPi and solder the wires

- 5V pin4 - Red wire
- GND pin6 - Black wire
- Tx pin8(GPIO14) - White wire
- Rx pin10(GPIO15) - Blue wire

***Make note that the Tx and Rx are crossed between devices.***

Mount the Pi however you would like
I just reused a screw from the screen and just mounted it where screen was until I can make something more permanant.

-------------------------------------------------------------------


Insert the SD card to the printers mainboard and power it on.

During the first 3 minutes the printer should be updating to the new firmware.

Make sure the the RPi is receiving power and the green light is flashing/solid.

On your PC, go to your web browser and type in the IP or domain of the RPi.

The mainsail dashboard should pop up.

WIP
















