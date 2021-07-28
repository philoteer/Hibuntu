# Alternatives: https://github.com/hexdump0815/imagebuilder , https://wiki.postmarketos.org/wiki/Google_Veyron_Jerry_Chromebook_(google-veyron-jerry)
# Original Project: https://github.com/com-py/Hibuntu ; Modified to run on the latest ChromeOS firmware
# Installs the mainline kernel, extracted from ALARM project, with Debian Buster userland
-----------------------------------------------------------------------------------------------------------------------
# Hibuntu
## Installing Debian on Hisense C11 ARM Chromebook (or to a veyron-jerry device, like Poin2 Chromebook 11)

### Alternative Instructions: Prebuilt image

A pre-built image for Debian Buster is attached at https://github.com/philoteer/Hibuntu/releases/tag/0.21 . Download it, and follow instructions in https://archlinuxarm.org/platforms/armv7/rockchip/hisense-chromebook-c11 (with ArchLinuxARM-veyron-latest.tar.gz replaced with the provided image) to create a bootable SD card or USB drive for a veyron Chromebook.

### Instructions (a harder way):

**The latest script is somewhat broken (mostly because I work on a armhf "desktop", and have not tested the script different systems); please switch to a tagged release and follow instructions there instead. **

The script automates installing Debian on Rockchip ARM chromebooks like the Hisense C11 (Veyron-jerry or similar platforms).
It is similar to ChrUbuntu but uses Arch Linux ARM kernel and Debian 10.
Either a microSD or USB stick can be used. I recommend the microSD option (16 or 32 GB)
because it fits flush on the Hisense C11 and can be very fast. You can have a dual-boot, full Linux
system on a nice, little and light machine like the Hisense C11.

**Requirements**: A microSD card or USB stick, wifi connection, and a Hisense C11 or alike (duh)

**1.** 	Set up developer mode and format your card/stick, following exact steps 1-8 given here:
  	
https://archlinuxarm.org/platforms/armv7/rockchip/hisense-chromebook-c11

**2.** 

On your Linux desktop, install dependencies and then download and execute the hibuntu script from this repository  https://github.com/philoteer/Hibuntu/blob/master/hibuntu, like:

```
sudo apt install debootstrap qemu-user-static 
sudo bash hibuntu ${target}
```

Where $target is the target drive, like /dev/mmcblk1 or /dev/sda . Be careful not to target a wrong drive, as it may damage your system.

**3.**
Next, make sure the device you want the OS to be installed on is the only external device plugged in.
Power the chromebook off then on, press `Ctrl-D` at OS verification screen, do not sign in yet.

Choose either 2a or 2b. I recommend 2a because it reduces ChromeOS interference.
It is assumed you have a microSD card. For USB, replace `/dev/mmcblk1` with `/dev/sda`. Also, you may have to replace replace `/dev/mmcblk1` to `/dev/mmcblk0` if you are running a firmware which puts the eMMC on mmcblk2.

### Post installation:

After reboot, press `Ctrl-U` to boot from the external device. 
If it beeps, just power off and on again. Make sure your device is the only one plugged in.
```
Username:  root
Password:  (empty)
```

### Tips:
To set up wireless after the first login, use these commands to scan, connect, 
and check connections; it's handy to jot these down:
```
nmtui
```
Wifi power save can cause connection drops. To turn it off, issue:
`sudo iw dev mlan0 set power_save off`

To make it permanent, save the following as an executable `wifipoff` file in the path `/etc/pm/power.d/`
```
#!/bin/sh
/sbin/iw dev mlan0 set power_save off
```
Once you have a connection, you can add more stuff. 
For example, to add the lxde desktop and assorted programs including firefox, do:
```
sudo apt-get install task-lxde-desktop
```
Finally, to check brightness, do:
`cat /sys/devices/backlight.20/backlight/backlight.20/brightness`

and to change brightness (replace 40 by any value 0 to 100):
```
sudo chmod 666 /sys/devices/backlight.20/backlight/backlight.20/brightness
sudo echo "40" > /sys/devices/backlight.20/backlight/backlight.20/brightness
sudo chmod 644 /sys/devices/backlight.20/backlight/backlight.20/brightness
```
You can put it in a script and bind it to a function key for easier use.

### TODO
- Fix Panfrost for Bullseye (possibly related: https://forum.armbian.com/topic/13515-panfrost-on-rk3288-and-gpu-on-600mhz-problems/ ). This does not really cause problems on Buster... probably because Buster does not really utilise the GPU much (because of the old Mesa version).
