# ISO installation
There are obviously many ways to install the ISO. Personally, I decided to torrent it because the concept of torrenting seems pretty cool. Anyways if you do decide to torrent it, please make sure that what you're downloading is actually what it says it is.  
(A quick way to verify before booting is just to check the signature or checksum before booting which can be found at ✨https://archlinux.org/download/#checksums✨)
After doing so, the set up process is pretty much similar to setting up any other VM (Do make sure, you've allocated enough ram and storage (I picked the default amount storage that virtual box recommended the first time and found out that it was in fact, not enough...))
## Booting into the installer 
Before being told that I should probably install arch via UEFI, I already started my installation using the BIOS instead so whoopsies on my part anywho,,,
    
    ip link
(to set up the internet, since we're on a VM, our wifi is recognized as a wired connection but if connecting using wifi use **iwctl**)
    
    timedatecttl set-ntp true
(keeps the time accurate (which btw is suprisingly important for when installing packages later..))

## Disk Partitioning 
Find your drive using:   
    
    lsblk

open your partioning tool of choice (in my case cfdisk and when asked about the label type pick dos (since we're doing an MBR install))
You’ll create two partitions:
1. A swap partition (about 4GB, or match your RAM if you want to be fancy).
   * Size: 4G
   * Type: Linux swap (that’s number 82 on the list)
2. A root partition that takes up the rest of the disk.
   * Type: Linux (number 83)
   * After creating both, move over to Write, type yes, and then Quit.

## Format & Mounting

    mkfs.ext4 /dev/root_partition
(That's our root partition)

Then make and enable swap:

    mkswap /dev/swap_partition
    swapon /dev/swap_partition

Mount the root partition 

    mount /dev/root_partition /mnt

## Installing the Base System

    pacstrap /mnt base linux linux-firmware nano

When the installation finishes, we generate the filesystem table (helps the system remember what's mounted where)

    genfstab -U /mnt >> /mnt/etc/fstab

## Chrooting Into the System

    arch-chroot /mnt

## Time Zone & Locale

    ln -sf /usr/share/zoneinfo/Region/City /etc localtime

    hwclock --systohc

uncomment out your language in /etc/locale.gen which in my case was

    en_US.UTF-8 UTF-8
then use:

    locale-gen
to generate the locales, then create a locale.conf file and set the LANG variable according to whatever language was picked above

## Hostname & Hosts
create your hostname in the /etc/hosname file, then edit the hosts file and make sure there are

    127.0.0.1   localhost
    ::1         localhost
    127.0.1.1   arch.localdomain (whatever your host name is)
## Installing Bootloader & Networking 
I chose GRUB as my bootloader of choice, i also decided that this is a great time to also install network manager for later use as well so:

    pacman -S grub networkmanager

And then since i'm doing a bios install, we have to install grub for BIOS systems:
    
    grub-install --target=i386-pc /dev/sda
    grub-mkconfig -o /boot/grub/grub.cfg
## Finishing Base Setup
Enable networking on boot

    systemctl enable NetworkManager

then exit, unmount, and reboot

## Post-Install: Adding a User & Desktop Environment 
Here's where I completed my first task of creating user accounts for myself and Codi so:

    useradd -m -G wheel an
    passwd an

    useradd -m -G wheel codi

Give them sudo priveleges:
    
    EDITOR=nano visudo

Uncommenting the line:

    %wheel ALL=(ALL) ALL

### Desktop Environment & Display Manager
I decided to download xorg per the wiki so we can run applications with a GUI later on but i also decided to use XFCE4 and with LXDM so:
    
    pacman -S lxdm xfce4 xfce4-goodies

and then enable LXDM so we can be booted into the desktop environment and then reboot

    systemctl enable lxdm.service

(Which also here would be a good time to mention that it would be a good idea to uncheck the arch.iso box next to the arch iso in the optical drive section on VirtualBox (That took me way too long to figure out why I simply could not boot right into my DE))

When LXDM appears, choose “XFCE Session” from the bottom-left corner and log in with your user.

## Final Step, Updates and Other tasks given

    sudo pacman -Syu

To check for updates

    sudo pacman -S openssh
    sudo packman -S zsh 

aliases (done by editing the ~/.bashrc file)

    alias cls='clear'
    alias update='sudo pacman -Syu'
















