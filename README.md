# Arch Linux Installation

**Standard Installation**

* **STEP 1**

    * Download Arch linux: https://www.archlinux.org/download/

* **STEP 2**

    * Make a usb bootable drive.
    
    My personal preference is using dd on a linux machine

    ```
    sudo dd if=/dev/sdb of=<path to the iso file> bs=512
    ```

* **STEP 3**

    * In the boot menu let it boot automatically into the first option

* **STEP 4**

    * Check for internet access 
    
    If connected via a LAN cable use enp0s3 ...

    ```
    ip addr show    

    ```

    * Accessing WI-FI...

    ```
    wifi-menu

    ```
    * Select your WI-FI network and enter the password

    * Check whether a ip address has been assigned to you via DHCP from your wireless network...

    ```
    ip a    

    ```

    * Check whether you have internet access by pinging google's DNS server...

    ```
    ping 8.8.8.8 -c 4

    ``` 


* **STEP 5** (OPTIONAL)

    * Edit your mirror list file incase of any connectivity timeouts...

    ```
    nano /etc/pacman.d/mirrorlist

    ```

    * Test the mirrors...

    ```
    pacman -Syyy

    ```

* **STEP 6**

    * Checking the disk structure...

    ```
    fdisk -l | more

    or...

    lsblk

    ```

    * Identify the disk where you'll install the OS....

    ```
    fdisk /dev/<your disk name>

    ```

    * Now on the fdisk prompt...
        In our case we'll use the EFI method and GPT partitioning scheme

    ```
    ##EFI PARTITION##

         g<cr>

         p<cr>

         n<cr> -Make a new partition

         <cr>

         <cr>

         +500M<cr> -This partition is used for efi and efi files

         yes<cr>

         t<cr>

         L<cr> -to get a list of all partition types

         1<cr> -Make our partition an EFI partition

    ##ROOT FILE SYSTEM PARTITION##

         n<cr>

         <cr>

         <cr>

         +30G<cr> -You can enter your preferred size

         y<cr>

    
    ##HOME PARTITION##

         n<cr>

         <cr>

         <cr>

         <cr> -Use the remaining space available

         y<cr>

    
    p<cr> -lists the partitions created

    w<cr> -Permanently writes the changes to your partitions
    
    ```



* **STEP 7**

    * Format our partitions...

    ```
    lsblk

    mkfs.fat -F32 /dev/<hard-drive<p1>>

    mkfs.ext4 -F32 /dev/<hard-drivep2>
 
    mkfs.ext4 -F32 /dev/<hard-drivep3>

    ```

* **STEP 8**

    * Mount our partitions...

    ```
    mount /dev/<root fs partition> /mnt

    mkdir /mnt/home

    mount /dev/<home partition> /mnt/home

    mount

    df -h  -Shows the mounted partitions

    mkdir /mnt/etc

    genfstab -U -p /mnt >> /mnt/etc/fstab

    cat /mnt/etc/fstab   -If everything was done correctly, then you should see your mounted partitions
    
    ```


* **STEP 9**

    * Installing base packages...

    ``` 
    pacstrap -i /mnt base  -Installs base packages

    arch-chroot /mnt  -Enter our in-progress arch installation

    pacman -S linux linux-headers linux-lts linux-lts-headers  -Installs both the latest linux kernel and the LTS kernel

    <cr>

    <cr> 

    ```

* **STEP 10**

    * Install additional packages...

    ```
    pacman -S nano  -Installs nano 

    pacman -S vim  -Installs vim

    pacman -S openssh base-devel

    <cr>

    <cr>

    systemctl enable sshd

    pacman -S networkmanager wpa_supplicant wireless_tools netctl

    pacman -S dialog

    systemctl enable networkManager

    mkinitcpio -p linux  -Generate initial ramdisk for the linux kernel

    mkinitcpio -p linux-lts

    nano /etc/locale.gen  -Choose your locale (Uncomment EN-US-UTF8)

    locale-gen

    passwd  -Enter the password

    useradd -m -g users -G wheel <username>

    passwd <username>

    which sudo  -Check whether you have sudo

    pacman -S sudo  -Install sudo if not installed

    EDITOR=nano visudo  -Sets the text editor to nano temporarily as we run the visudo command (uncomment wheel ALL=(ALL) ALL inorder to enable our users in the wheel group to use the sudo command)    
    
    ```

* **STEP 11**

    * Setting up Grub in order to boot the linux system

    ```
    pacman -S grub efibootmgr dosfstools os-prober mtools

    mkdir /boot/EFI

    mount /dev/<hard-drivep1> /boot/EFI

    grub-install --target=x86_64-efi --bootloader-id=grub_uefi--recheck

    mkdir /boot/grub/locale

    cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo

    grub-mkconfig -o /boot/grub/grub.cfg

    ```


* **STEP 12**

    * Making a swap file...

    ``` 
    fallocate -l 2G /swapfile  -Creates a swapfile

    chmod 600 /swapfile

    mkswap /swapfile

    cp /etc/fstab /etc/fstab.bak  -Make a backup of the fstab file

    echo '/swapfile none swap sw 0 0' | tee -a /etc/fstab

    cat /etc/fstab  -Now you should see the root, home and swap partitions
    
    ```

* **STEP 13**

    * Installing optional packages

        
    ```
    
    pacman -S intel-ucode  -Installs the cpu microcode

    pacman -S xorg-server  -If you plan on using a DE

    pacman -S mesa  -Installs graphics drivers

    pacman -S virtualbox-guest-utils xf86-video-vmware  -If installing arch on a virtual machine 
    
    ```


* **STEP 14**

    * Moment of truth...

    ```
    exit

    umount -a  -It's okay if you see some errors

    poweroff<cr>

    -Power on the machine
    ```



























    
