---
date: 2019-02-25
author: KarolNi
tags: SMART ASM1053 UAS smartctl linux Seagate STAE-104 USB3.0 0bc2:a013
---

# SMART over ASM1053

Couple days ago I had to check smart of an external HDD, but it did not work this time. It was flawless last time, I even did not have to manually specify protocol. This time it did not.

My adapter is USB3.0 Seagate STAE-104. It uses ASM1053 chip. It's USB vid and pid is 0bc2:a013.

Turns out that couple kernel updates and my adapter now works in UAS mode (see https://en.wikipedia.org/wiki/USB_Attached_SCSI). The issue is that UAS on ASM1053 is little buggy - SMART does not work (it also has stability issue - my system froze two times on disconnecting USB). To fix it you need to tell the kernel to ignore UAS mode for this device and use legacy MSC mode. For hot fix (without rebooting):

'''
# unmount and disconnect all USB storage devices
sudo rmmod uas 
sudo rmmod usb-storage
sudo modprobe usb-storage quirks=0bc2:a013:u # you can put here your's device vid and pid, if you have issue with other device
'''

Now SMART is read flawlessly with smartctl command.

UAS mode is marginally faster in my test - it scored 277MBps and legacy mode 255MBps.

