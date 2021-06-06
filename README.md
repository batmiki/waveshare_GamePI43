# waveshare_GamePI43

This is a short description on how to adapt the Recalbox 7.2.x configuration to match the Waveshare GamePi43 button mappings. It is completely based on the information available on [Waveshare's Wiki page][1]. 

## Motivation
The [Waveshare's Wiki page][1] already describes in a very good way what to do in order to install the GamePi43 drivers and setup it on both RetroPie and Recalbox platforms. Well, for the latter, the latest supported version they mention is 7.0.1, whereas the most recent Recalbox version at the time of writing this text is 7.2.1. The provided by Waveshare modified kernel object file does not match the kernel version of this recent Recalbox. Fortunately, this file seems not to be neded any more and just slight extension of the manual edit step Waveshare describes for Recalbox 7.0.1 is needed to make this great gaming box again alive.

## How to...
The below instrcutions are based on the [Waveshare's Wiki page][1] "how to" section "Install driver on Recalbox", but omits any steps installing something on the Recalbox. Instead, the mapping will be manually setup, since it is a two line thing.

There are two options you have:
1. Edit the configuration file within the Recalbox platform itself

```shell
mount -o remount,rw /
mount -o remount,rw /boot

#Open the configuration file for editing
nano /recalbox/share/system/recalbox.conf
```
Then move to step 3.

2. Edit the configuration file from another system 

If you take this way, then just mount the SDCARD with the Recalbox SHARE partition and open for editing the {MountPoint}/system/recalbox.conf, e.g. for a Linux system do: 

```shell
# Check which device and partion to map (if not yet mounted): 
lsblk

NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 238,5G  0 disk 
├─sda1   8:1    0   512M  0 part /boot/efi
├─sda2   8:2    0   237G  0 part /
└─sda3   8:3    0   977M  0 part [SWAP]
sdb      8:16   1  29,7G  0 disk 
├─sdb1   8:17   1     3G  0 part 
├─sdb2   8:18   1  25,7G  0 part 
└─sdb3   8:19   1  1023M  0 part 

# Device sdb is the SDcard here -> sdb2 is the SHARE partition:
sudo mount -o rw /dev/sdb2 /mnt
sudo nano /mnt/system/recalbox.conf
```
Note:
 If the lsblk output looks like this:
```
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 238,5G  0 disk 
├─sda1   8:1    0   512M  0 part /boot/efi
├─sda2   8:2    0   237G  0 part /
└─sda3   8:3    0   977M  0 part [SWAP]
sdb      8:16   1  29,7G  0 disk 
├─sdb1   8:17   1     3G  0 part /media/batmiki/RECALBOX
├─sdb2   8:18   1  25,7G  0 part /media/batmiki/SHARE
└─sdb3   8:19   1  1023M  0 part /media/batmiki/OVERLAY
``` 
then the partiions are already mounted, so you do not need to mount anything else!

Now move to step 3.

3. Edit configuration file

Once you have opened the file for editing, the following lines must be adapted or added if still commented out:

```
controllers.gpio.enabled=1
controllers.gpio.args=map=1 gpio=17,7,27,22,10,9,25,24,23,15,18,14,2
```

And that was it. Reboot your Recalbox and enjoy playing games.

Note: It is still the same effect observeable as with the older Recalbox version where the Waveshare's installed driver was used: the A and B buttons are swapped within the Recalbox menu. 


[1]: https://www.waveshare.com/wiki/GamePi43
