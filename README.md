# Live_Ubuntu_Persistent_FD
Setup a Live Persistent USB Flash Drive.

Unetbootin

Use Unetbootin to write to the FD. Download and install info from https://unetbootin.github.io/linux_download.html, or just use this repository:

sudo add-apt-repository ppa:gezakovacs/ppa

sudo apt-get update

sudo apt-get install unetbootin

then:

neill@LV510:~$ cd ~/unetbootin

neill@LV510:~/unetbootin$ sudo QT_X11_NO_MITSHM=1 /usr/bin/unetbootin

Comment: [I'm using Ubuntu 18.04 with Xorg, and the unetbootin window is showing blank window when I tried to run it via dash. But whe I type sudo QT_X11_NO_MITSHM=1 /usr/bin/unetbootin, it worked just fine]

Method:
     
    
    1. Use at least a flash drive with 16Gb of space on it;
     
    2. Format the first partition (sdb1) to FAT32 of at least 10Gb for Ubuntu 18.04 (or other OS)
    
    3. Format a partition (sdb2) of the remaining space (>4Gb) named casper-rw  to Ext4;
    
    4. Mount flash drive under /media/username;
    
    5. Run Unetbootin to load from a local .iso file;
    
    6. Set Persistance in Unetbootin to store settings to 4000Mb;

Then:

Edit filesystem cdrom/boot/grub/grub.cfg by adding "persistent" as shown below. Do this from another PC to gain access to the filesystem. 
(set relevant permissions with  ~$ sudo chown user /media/username):

Code:

file=/cdrom/preseed/ubuntu.seed boot=casper persistent quiet splash --

Next:

Replace ALL the contents of syslinux.cfg with:

Code:

default persistent

label persistent

  say Booting an Ubuntu persistent session...
  
  kernel /casper/vmlinuz.efi
  
  append  file=/cdrom/preseed/ubuntu.seed boot=casper persistent initrd=/casper/initrd.lz quiet splash noprompt --
  


QED!



Notes and Info:

From https://askubuntu.com/questions/229808/if-i-boot-from-a-usb-drive-will-i-still-be-able-to-use-my-usb-drive-for-other-th/229837

If you do a persistent install your first partition will be Fat32. Both Ubuntu and Windows can read and write to FAT32.
When booted from a persistent install, the data on the flash drive can be found in filesystem/cdrom. You need to be Root to access it.

A full install of Ubuntu on flash drive can be made with the first partition being FAT32 or NTFS and / on a following partition. You can access this first partition without being Root.

If Gparted is not showing, type in:     neill@LV510:~$  xhost +local:root
