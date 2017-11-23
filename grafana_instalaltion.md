# Grafana Setup on raspberry

## Prerequisite
* Internet Connection (can be WiFi or Ethernet)
* 16+ GB micro SD card with fast write/read 80mb/s+
* Raspberry Pi 3 with charger (can be connected to the computer by micro USB cable)
* Computer screen with HDMI cable
* Mouse and keyboard

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

## Things to do after Raspbian installation
First you should start with updating the system. Open the terminal window and paste
```bash
sudo apt-get update && sudo apt-get upgrade
```
It will start to check and download new version of existing packages.

### RDP and SSH


```
sudo apt-get install xrdp
```

### MySQL installation
When you finish apt-get update and upgrade it is time to install MySQL.
```bash
sudo apt-get install mysql-server python-mysqldb mysql-client
```
During the installation you should be prompted to set root password and etc. If you have not receive this notification in the terminal paste. It is recommended to turn off remote access for root and delete test databases. After this you need also to reload privileges.
```bash
mysql_secure_installation
```
To access MySQL db simply write in terminal
```bash
mysql -u root -p
```
and next type the root password that you set in previous step. Now you are in the MySQL shell (probably MariaDB). Now let's break into it.


### phpMyAdmin
After setting MySQL to access and manage it with nice UI, it is recommended to install phpmyadmin. To do it please type in the terminal
```bash
sudo apt-get install phpmyadmin
```
During the installation you will be prompted to chose between apache and light-httpd. Chose apache. Next you will be asked to configure db for phpMyAdmin, chose yes. Next screen will ask you to type root password to you MySQL and confirm it once again.


## Graphana

msyql user 'grafana-reader'@'localhost' 'rpi'
