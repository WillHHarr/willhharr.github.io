# arch.linux.install

Acquiring a 2026.10.01 ISO distribution of Arch Linux, I choose to utilize ProxMox to create an Arch VM.

1.) Generating the Proxmox VM profile
In generating the Proxmox VM, we gave the VM the latest 2026.10.01 ISO, 2 cores, 2 GB ram, and 32GB of disk space

2.) Booting the VM
We then proceed to boot the VM up inside proxmox, then once running connect into the Proxmox vnc to go through the arch linux install.

3.) Confirming boot mode
By running **cat /sys/firmware/efi/fw_platform_size** we are able to verify we are in BIOS mode without a proper VM instillation currently installed.

4.) Confirming Clock
To prevent desync issues later on, we will run **timedatectl** to verify that we have the correct time and date sync.

5.) Partitioning Disk
We will run **fdisk -l** to list out the locations of the physical disks present in our enviroment. We will then run **fdisk [insert disk path]** to enter the fdisk util. We will create 3 partitions of sizes 1GB, 4GB, and 27GB correspondingly with types EFI, Linux Swap, and Linux.

6.) Mounting disks
We now need to mount all 3 partitions to enable full instillation. We will mount our SDA1 (1GB) to /mnt/boot, enable SDA2 (4GB) with swapon, and mount our SDA3 root (27GB) to /mnt.

7.) Installing Arch
Now we need to actually install ArchLinux to our device's storage. This can be done by running the command **pacstrap -K /mnt base linux linux-firmware** to gather the base packages needed for arch linux to work.

8.) Configuring system
Next to configure this system after we download it. 
First, we need to make some fstab files and copy over some boot EFI files we generate from deploying the limine bootloader into your boot partition. 
We then need to edit some bootmgr commands, using **efibootmgr --create --disk /dev/sda --part 1 --label "Arch Linux" --loader "\\EFI\\Linux\\arch-linux.efi" --unicode** to set the bootloader to now link to our newly installed OS. We then reboot the device.

9.) Booting to OS
Now that we have an OS installed on our device, we now need to change the boot order of the devices, from our image to our drive. After we have done this in proxmox, we now restart our device and boot into our OS.

10.) Setting Up Accounts
First, we use the **useradd -m -s /bin/bash [username] command** to create the accounts codi and user.
Running **passwd [username]** to assign a password.
Then using **usermod -aG wheel [username]** to assign sudo permissions.
also installing and enabling the sudo package from packman to give **wheel** group sudo permissions.

11.)Installing a new shell
I am installing the zsh shell. To do so I run the pacman install command using the title "zsh". I then run the command zsh to be able to utilize this new shell.

12.) Installing SSH
You then install your SSH tunneling capability with pacman's openssh package. You then start and enable sshd with the systemctl utility. 

13.) Terminal colors
Adding colors to the zsh shell, I download the grml-zsh-config file, and copy that configuration file into my zshrc config file. Setting this shell as the default as well.

14.) Desktop enviroment
Given the system we are utilizing is a qemo agent, we need to install a large number of packages for this to work.
First, we need to download the actual desktop enviroment files itself. 
First, these packages are installed **networkmanager pipewire pipewire-pulse wireplumber**.
We then install LXQt itself from the packages **lxqt openbox breeze-icons sddm qterminal pcmanfm-qt nm-tray pavucontrol-qt**.
We then have to install a qemu agent, so these packages are installed **qemu-guest-agent xord-server xf86-video-fbdev xf86-video-vesa**. Enabling **systemctl enable --now qemu-guest-agent**.
LightDM and GTKgreeter are installed as well, **lightdm lightdm-gtk-greeter** enabling lightdm and disabling sddm. **openbox lxqt-session** are also installed to help as a window manager.
**sudo systemctl start lightdm** is now ran to set a display target. The desktop enviroment is now shown.
