## Raspbian instalation
There are common issues with the raspberry pi 3 regarding built in Wi-Fi adapter. Thats why it is recommended to use [NOOBS](https://www.raspberrypi.org/downloads/noobs/) option.

### How to install NOOBS
1) Download the file from [here](https://www.raspberrypi.org/downloads/noobs/). For this setup it is recommended to use version that contains Raspbian.

2) Unzip the archive.

3) Using tool like for example [SD Card Formatter](https://www.sdcard.org/downloads/formatter_4/index.html). It has version for macOS and Windows. By default it formats to FAT32, but if you have used 64+ GB SD card it will format it to exFAT which is not supported. If you have this problem please follow [this instruction](https://www.raspberrypi.org/documentation/installation/sdxc_formatting.md).

4) Copy all the files that were in the NOOBS archive to the micro SD card.

5) When this process has finished, safely remove the SD card and insert it into your Raspberry Pi.

6) Plug screen, mouse and keyboard to Raspberry and then plug the power supply. Raspberry should start to boot now.

7) When you are prompted to chose operating system, check Raspbian. At this stage you can also connect to the WiFi or plug ethernet cable to Raspberry Pi. Proceed with the installation and after few minutes you should be all set.
