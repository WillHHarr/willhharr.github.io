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
