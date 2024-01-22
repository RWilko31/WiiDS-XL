# WiiDS-XL
A portable Wii created in the form factor of the 3DS XL.

Overview:
This project is a work in progress.

This project aims to take a Nintendo Wii motherboard and fit it in a DS formfactor along with the necessary parts to create a full portable.

Goals:
- Use a real DS shell (currently 3DS XL) although modification is allowed
- use the standard Wii trim to minimise difficulty
- Have a battery life of at least 2 hours
- USBC fast charging

Desirables:
- have the device be dockable to allow connecting to a TV, ability to use GC controllers/memorycards
- Keep wifi and bluetooth in the portable
- add the disk drive to the dock to allow disk games to work
  
Current progress:

Many wii portables have been made before, and so the process is very well documented with a full guide being available to show the steps needed to make it work:
Wii trim guide - https://bitbuilt.net/forums/index.php?threads/the-definitive-wii-trimming-guide.198/ 

Although smaller trims are possible, they are much more involved as they require relocation of the memory and video ICs.

Power supply:
Power guide - https://bitbuilt.net/forums/index.php?threads/custom-regulators-an-explanation-and-guide.754/ 

Power requirements: 3v3 @ 0.66A, 1v15 @ 1.8A, 1v @ 1.53A
additional: 5V for usb/GC rumble, 1v8 (Reg still on board when trimmed)

To power the wii at least 3 regulators will be required to supply 3v3, 1v15 and 1v. The trimmed wii will maintain the 1v8 LDO regulator which draws from the 3v3 supplied. 
Since the DS will not have a USB port the 5v can be removed and supplied by the dock instead.

Battery and battery management:

A portablised wii is estimated to draw around 10W
https://bitbuilt.net/forums/index.php?threads/wii-portable-total-power-draw-mah.2368/#:~:text=A%20basic%20Wii%20portable%20with,about%2010%20watts%20of%20power.

The battery time can be found with the following equations:

Cell V * (mAh / 100) = WH
WH / W = Battery life in Hours

To maximise on efficiency the battery voltage will be at 3.7v which is the average LiPo cell voltage.
To find the minimum capacity we can use the play time of 2 hours and rearrange to find capacity:

2*10 = 20WH
(20/3.7)*1000 = 5405.4 mAH

This means to reach the minimum playtime set out we would require a battery above 5405 mAH. 
Looking at form factor and battery capacity it seems likely to be able to fit a 10000mAh call.

this would provide a play time of:
3.7*(10000/1000) = 37WH, 37/10 = 3.7 hours

Management IC:
The inspiration for the power section of this project is this board:
https://4layertech.com/products/rvl-pms-2

The board provides the voltages required and battery management. This board would be ideal however the cost is quite high and so instead the board will be used as a basis to recreate a version more tailored to the project.
The battery management IC is a bq24292i.
https://www.ti.com/lit/ds/symlink/bq24292i.pdf 

This is a single cell battery manager capable of 4.5A charging, with I2C to allow changing charging and system settings.
The datasheet provides an example schematic which would be ideal for this project, however a development board is also available which provides a slightly altered layout which could be used as well:
https://www.ti.com/lit/ug/sluua14c/sluua14c.pdf?ts=1705785263286&ref_url=https%253A%252F%252Fwww.ti.com%252Ftool%252FBQ24292IEVM-021

USBC interface:
In order to charge the battery via USBC, a USBC interface is required to communicate with the charger to request the correct voltage and current.

HUSB238 USB Type-C Power Delivery Sink Controller
https://www.hynetek.com/uploadfiles/site/219/news/aabbbbdb-48c9-4a44-a6dc-2c15f53282e6.pdf

This IC has a simple schematic and is capable of requesting any usbc voltage and current via resistors to set its values.


