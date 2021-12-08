Wiki for the software and hardware running HWGC P&P machines 
======
## Software section
The main program behind the P&P machine is a Windows Form application built with Visual Studio 2010, of which interacts and controls all parts of the hardware. 
It can controll all of the HWGC machines from the 4 head 38 feeder all the way up to the 8 head 120 feeder machines. 

The only languages it supports are Chinese and broken English. The languages are hard coded in the software but more can painstakingly added.
Without the hardware attached, it has very limited use outside of chaning langauge and a few settings. For full operation of the software, you must connect to the controller boards and camera capture systems.

The software has licensing locks to prevent users from using the software without a licence. Without entering a key, users can run the software but only run
* Auto mode runs 5 board at once
* 150 IC chip import max
* 5 component optmisation limit
* Must restart application after 10 boards
* Total chips placed limit of 120,000

Based off the source code, there are multiple licences variations.
Either a perpetual licence that unlocks the machine for life, or one based on the date or how many chips you place.

The licence looks to be partly generated off hardware IDs which includes the network adapter (MAC address) and physical media serial numbers (HDD serial number). 
The registration page generates a long "Device Code" made up of the Processor, Motherboard, HDD and BIOS serial numbers plus a final 28 digit number based off the MAC address and HDD serial that is ran through some sort of algorithm to generate the number.

While the Windows Forms part of the software has been decompiled and made avalible. 
The .DLLs are either not C# or have been obfuscated, making it impossible to decompile to source. 

Many functions are within the DLLs are for licencing, machine movement, most if not all the visual algorithms etc.
That being said. The work done on this Github is not to reverse engineer the code and make the valuable assets avalible to all.
Only to make it better and easier for those who own the machines to run especialy as it has arbitrary hardware locks.

#### Software Requirements
Drivers are needed for a few of the devices such as the Capture Cards, USB to Serial and High Resoltuion USB3 camera.

For the Windows Forms application and DLLs, you will need 
* Microsoft .NET Framework 4.5 https://www.microsoft.com/en-gb/download/details.aspx?id=30653
* Microsoft Visual C++ 2010 Redistributables both x86 and x64 https://www.microsoft.com/en-gb/download/details.aspx?id=26999



## Hardware section

### Host Computer
* Intel 8th or 9th gen CPU
* MACMEMORY X100 120GB SSD
* GIGABYTE H310M HD2 2.0 motherboard
* Single stick of DDR4 RAM

### P&P Controller boards
Unkown but source code is looking for CP210x USB to Serial devices with hardware names that include "HWGC-QiGn".
Going off photos and video from HWGC, it is highly likely that they are manufacturing these boards themselves and can only be sourced via them.



### Camera hardware and capture
The machines make use of multiple cameras and capture systems to image the PCBs and components. 
This is split into the Fast Cam (component camera)/Mark Cam (fiducial camera) and High Cam (high resolution camera for larger components). 

#### Fast/Mark Cameras
The actual camera hardware is unkown at this point. 
The source code sugests that the drivers are for Jinan Jovision PCI 4, 8 and 16 channel PCI DVR capture cards but photos of the internal computer suggest that is might not be correct. Waiting on more info.

#### High Res Camera
Based on the .DLL and source code, we know that the machines are using a CatchBest U3C500M/C USB3 camera for the high resolution camera that takes photos of larger components. 

![1593657963](https://user-images.githubusercontent.com/1049919/145130968-2e516359-f7e9-4860-ac3a-bb014b1cfc29.jpg)

The camera is a CMOS image sensor with a resolution of 2592x1944 and a frame rate of 30 FPS. Though the software only makes use of 1944x1944 pixels for image processing.
This camera is listed on the manufacture’s website and is still available for purchase via Alibaba and other China based sourcing websites.
http://www.catchbest.com/product/25-cn.html


### Motors and Drivers

#### Motors

#### Drivers

### Pick and Place components











