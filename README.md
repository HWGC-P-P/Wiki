Wiki for the software and hardware running HWGC P&P machines 
======
#### EEVBlog discussion forum: https://www.eevblog.com/forum/manufacture/yx-smt660-smt880-pp-machines/
#### Discord server: https://discord.gg/FxhdJNVWbB

## Manufacturing and ordering Details
These machines are produced by
  * Beijing Huawei Guochuang Electronic Technology Co. Ltd.
  * Website: http://www.smthw.net/lang/en.
  * Alibaba: https://zghwgc.en.alibaba.com
  * Contact: Liu Jiangtao, MR.zhai

They are sold by many resellers, some of which rebrand the machine names
  * Beijing Glichn 
  * Beijing Huawei Silkroad Electronic Technology Co. Ltd
  * Wenzhou Yingxing Technology Co.,Ltd
  * Robodigg

HWGC's naming tends to be "HW" for Huawei - Heads - Feeders
  * HW-T8SG-80F 
  * HW-T6-64F 
  * HW-T4-50F

Shipping
  * These systems are large and heavy; they usually ship by sea.  
  * If not explicitly called out as "door to door delivery", assume shipping rates are "to the nearest seaport".
  * If you have never imported an item like this, consider engaging a customs clearance company or importer who can navigate your country's import, tariff and tax filing requirements.  Expect to pay $100-$200 for the service plus any government fees and taxes.


## Workflow
The supplied software is designed for production workflows, and not hobbiests and prototyping.
That is, it requires a significant time investment in job set up and machine pre-configuration, with the goal of reducing the time needed to actually produce large quantities of populated boards.  This workflow can be broken down into the followiong steps:
  1. Create a Project, usually by importing a centroid file from the PCB CAD toolchain
  2. Optionally add support for a panelized design
  3. Associate the design's components with feeders and nozzels
  4. Associate the Project with a Board using fiducial markings on the board
  5. Load the required components into feeders/trays/tubes
  6. Mount feeders (...) onto the machine in the locations specified
  7. Load the required nozzles onto the heads
  8. Calibrate the feeders using the downlooking camera
  9. Populate a test board, check placement and rotation of each component, fix and repeat as necessary
  10. Finally, press Auto Run and start loading/placing boards...

## Software section
The main program behind the P&P machine is a Windows Form application built with Visual Studio 2010, of which interacts and controls all parts of the hardware. 
It can control all of the HWGC machines from the 4 head 38 feeder all the way up to the 8 head 120 feeder machines. 

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

The source code sugests that the drivers are for Jinan Jovision PCI 4, 8 and 16 channel PCI DVR capture cards from around 2007-2011.
Photos of the internal computer show a PCI-E capture card with an Altera Cyclone® IV FPGA connected to an Nextchip NVP6114 AHD 4 channel CVBS/COMET/AHD1.0 decoder. 
![image](https://user-images.githubusercontent.com/1049919/145662007-01de9c46-d295-4b6e-b3b4-07f50477d83f.png)
The Jovision cards use an old Sony package to do the AHD decode and compression but these chips are circa 2007 and are EOL which could explain why these boards are using an FPGA long with a CVBS/COMET/AHD1.0 decoder chip to support backwards compatabilty with older systems/software that uses these drivers. [speculation]



#### High Res Camera
Based on the .DLL and source code, we know that the machines are using a CatchBest U3C500M/C USB3 camera for the high resolution camera that takes photos of larger components. 

![image](https://user-images.githubusercontent.com/1049919/145662245-619da54c-31f3-494f-a747-542d75115bed.png)


The camera is a CMOS image sensor with a resolution of 2592x1944 and a frame rate of 30 FPS. Though the software only makes use of 1944x1944 pixels for image processing.
This camera is listed on the manufacture’s website and is still available for purchase via Alibaba and other China based sourcing websites.
http://www.catchbest.com/product/25-cn.html


### Motors and Drivers

#### Motors

#### Drivers

### Pick and Place components


#### Electric Feeders (Yamaha CL 8*4)
  * You need to explicitly ask for a machine upgrade to support electric feeders, as they default to pneumatic only.
  * With this upgrade, the machine supports the use of both electric and pneumatic feeders.
  * Electric feeders are required for reliable placement of very small components 
    * Use Pneumatic for 0603 and larger
    * Use Electric for 0603 and smaller
  * https://offer.alibaba.com/cps/asciqch7?bm=cps&src=saf&productId=62431099223










