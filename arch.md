<link href="/css/style.css" rel="stylesheet">

[Home](index.md)

## Welcome to my guide on installing the best Linux distro FOSS can buy. Lets get started!
* [Arch Linux Download](https://www.archlinux.org/download/)
* [Virtualbox Download](https://www.virtualbox.org/wiki/Downloads)


* First things first, we need an install environment. For this, Virtualbox will suffice.

* Now, install Virtualbox and Create your VM.

![](images/Virtualbox 1.png)

* I recommend having allocating memory between 2-4GB

![](images/Virtualbox 2.png)

* Create your HD image. Choose your favourite format. Default settings will suffice and choose how much space you want to allocate. I recommend a minimum of 30GB for VM's.

![](images/Virtualbox 3.png)

* Now after the VM is created, tweak it to your liking, I recommend having 2-4 CPU cores allocated, and attach your Arch Linux ISO file to your VM. This is where the fun starts.

* Boot up the VM and boot Arch Linux from the selection.

![](images/Virtualbox 4.png)

* Wait a little and you'll be greeted with a command shell. Now follow these steps carefully. The first thing we need to do is partition the HD. I recommend using the `cfdisk` utility. If you only have one HD attached, simply run `cfdisk` on its own. Extra hard drives can be partitioned by running `cfdisk /dev/sdX`with the "X" being the drive letter. Eg `cfdisk /dev/sdb`.

![](images/Virtualbox 5.png)

This is a typical partition setup for a standard boot, I will cover UEFI [here](archefi.md), it is best to have a separate boot partition from the root. With applying partitions, select **New** and type in the size you want, the recommended size is 500M for the first boot partition. So just type in `500M` and press enter and set it as primary. The next partition is for swap, swap is basically temp storage for when memory runs out. Kind of like a pagefile in Windows. I recommend having around `2G` for the second partition and apply it. Then select **Type** then select **82 Linux Swap/Solaris**. The final partition is the root partition. Just press **New** and fill in the rest of the drive by just pressing Enter right through. Then simply hit the **Write** selection to write this partition table. Then quit out. Also, take note of your partition numbers so we can format them accordingly.

* Now we come to formatting the partitions on the drive. The simplest and most reliable being EXT4. You can view all formats by typing `mkfs.` then hitting the TAB key. It will list all available `mkfs` commands. This small trick applies to all commands and directories, pressing TAB speeds up a lot of boilerplate typing.

![](images/Virtualbox 6.png)

Now run these commands in order, I'll be using my `/dev/sda` so yours might be different depending on your setup, so double check if you have multiple drives on an actual machine. Note: The swapon command below mounts the swap partition for use.

```
mkfs.ext4 /dev/sda1

mkswap /dev/sda2

swapon /dev/sda2

mkfs.ext4 /dev/sda3
```

* Next is mounting the partitions. So simply type this:

```
mount /dev/sda3 /mnt
```
You just mounted `/dev/sda3` to the `/mnt` folder. Next we have to make a **boot** folder to mount our boot partition on, so simply type in:
```
mkdir /mnt/boot
```
Then we need to mount our drive partition onto it:
```
mount /dev/sda1 /mnt/boot
```
Success! We now have mounted our drive and ready for installing Arch to it.

* Installing Arch isnt so hard after all, is it? Simply now run:

```
pacstrap /mnt base
```
This command boostraps the Arch Linux files to your drive. Simply wait till the command is done. After it is complete, we need to run:
```
genfstab -U /mnt >> /mnt/etc/fstab
```
This generates the FSTAB(FileSystemTABle) for partitions to be mounted on boot.

* Now we can `chroot` into the new install of Arch. Simply run:

```
arch-chroot /mnt
```

* Now that we're chrooted in, we need to set our locales. Run this command:

```
nano /etc/locale.gen
```
And then uncomment out your locale, most being `en_US.UTF-8`, save then exit by pressing `ctrl-x` and run:
```
locale-gen
```
Next is to write a `locale.conf` file. Run:
```
nano /etc/locale.conf
```
And in this file, type in `LANG=en_US.UTF-8`, save and exit.

* Next up is our hostname, just similar to the commands above, run:

```
nano /etc/hostname
```
Type in what you want for a hostname, which is basically the computer name, save and exit.

* Now we can install our bootloader and network utility. Run:

```
pacman -S grub networkmanager
```
Then we need to enable the Network Manger so it starts on boot, run:
```
systemctl enable NetworkManager
```
Now we can install the bootloader, simply run:
```
grub-install /dev/sdX
```
Note that to replace `X` with your drive letter. E.G `/dev/sda`

We're almost done, don't panic. Now just run:
```
grub-mkconfig -o /boot/grub/grub.cfg
```
That generated your Grub config file to boot your Arch install. Next thing is to run:
```
passwd
```
This sets the root users password. Type in a password and press enter.

And that completes the install. Enjoy your new Arch Linux. Run:
```
exit
```
This exits out of the chroot. Then run:
```
umount -R /mnt
```
To unmount the disk, then simply type in:
```
reboot
```

Now I will cover installing a desktop environment [here](archdesktop.md)
