# Pre-installation

Downloaded the image for the arch VM from the downloads link in the Install Guide on ArchWiki

Verified the hash.

Opened the image in VirtualBox, the program I am choosing to host my arch vm on.

Booted into the system, but first booted into BIOS so then rebooted correctly in UEFI mode. There was a setting that needed to be checked to boot into UEFI

```cat /sys/firmware/efi/fw_platform_size```
returned 64 meaning properly booted into UEFI mode

checked the network connection by run this command:
```ping ping.archlinux.org```

checked that the system clock was enabled by run this command:
```timedatectl```

```fdisk -l```
```fdisk /dev/sda```
p to check current partitions (none)
n to make new partition > 500 MB for EFI
n to make new partition > rest of drive (7.5 gigs) for Arch OS
t to change the type of partition of sda1 to efi
w to write and save changes

sda1 > EFI
sda2 > rest of the drive

```mkfs.fat -F 32 /dev/sda1```
Format the first partition for the uefi boot

```mkfs.ext4 /dev/sda2```
Format the second (the home partition) the create the ext4 file system

```mkdir /mnt/mount```
create the directories for the file systems to mount to

```mount /dev/sda1 /mnt/mount/boot/efi```
```mount /dev/sda2 /mnt/mount```
mount both partitions 

```lsblk```
this checks that everything was mounted correctly

# Installation 

```pacstrap -K /mnt/mount base linux linux-firmware```
This command installs the base system, the linux kernel, and firmware

# Configuration

```genfstab -U /mnt/mount >> /mnt/mount/etc/fstab```
```cat /mnt/mount/etc/fstab```
This command generates the fstab to get the file systems mounted on startup. The second command is double checking that everything was correctly mounted and labeled

```arch-chroot /mnt/mount```
this command allows us to interact with the system as if we booted into it

```ln -sf /usr/share/zoneinfo/central/Tulsa /etc/localtime```
setting the time zone

```hwclock --systohc```
assumes the hardware clock is set to UTC (coordinated universal time)

```locale-gen```
generating the locales

```pacman -S nano```
install nano to be able to edit files
(-S synchronizes and installs)

```nano /etc/locale.conf```
creating the locale.conf file
set the LANG variable to LANG=en_US.UTF-8

```nano /etc/hostname```
create a hostname file in the /etc directory
my host name is now caroline

```passwd```
create a password for root privileges (p: arch)

```pacman -S grub```
```pacman -S efibootmgr os-prober```
install both grub and efibootmgr to finish install and set up booting into personal machine instead of live machine

```grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB```
```grub-mkconfig -o /boot/grub/grub.cfg```
create the boot loader configuration file for proper booting 

(really struggled with this part. I did not correctly mount the boot loader the first time so i got stuck in a loop and couldn't access the command, so I created a new VM and ran through these steps a second time and had success)

```pacman -S dhcpcd```
install this in arch-chroot so it can be used to make a network connection on the installed machine (I also really struggled here because I couldn't figure out how to get a network connection in my vm, so I backtracked to here)

exit arch-chroot

reboot system and relaunch using the grub boot loader

# Class_Requirements

```ip link set enp0s3 up```
```dhcpcd enp0s3```
connect to the internet in my installed machine

```pacman -S xorg-server sddm lxqt openbox```
installed a desktop environment

```useradd -m -G wheel -s /bin/bash caroline```
```useradd -m -G wheel -s /bin/bash codi```
```passwd caroline```
```passwd codi```
created to users with root privilege and assigned passwords

```sudo EDITOR=nano visudo```
change access on the wheel group so members of said group have sudo privileges

```pacman -S zsh```
```chsh -s /bin/zsh caroline```
installed zsh shell and changed the user caroline's default shell to zsh (I did change it back to bash by replacing the zsh with bash)

```pacman -S openssh```
```systemctl enable sshd```
```systemctl start sshd```
```systemctl status sshd```
installed openssh and double checked that it was running

```alias ls='ls --color=auto'```
```alias grep='grep --color=auto'```
change ls and grep function to be color coded in the terminal in the zsh shell

```force_color_prompt=yes```
change to add color to terminal in bash

```sudo systemctl enable sddm.service```
set the vm it boot into the desktop environment 

to add alias, I used nano to edit the .bashrc file. Some of the aliases I added where when 'c' is entered, the terminal screen clears.

# Screenshots
![[20CF309A-9235-477D-982D-D39DB5BF46B3_4_5005_c.jpeg]]![[IMG_0451.heic]]![[IMG_0450.heic]]
