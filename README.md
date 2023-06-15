# Kali-Linux-8892eu Drivers


    Install DKMS and other required tools for normal Linux systems

    sudo apt-get install git linux-headers-generic build-essential dkms
    
    for Raspberry Pi

    sudo apt-get install git raspberrypi-kernel-headers build-essential dkms


    Clone this repository and change your directory to cloned path.

    git clone https://github.com/munzoorf95/Kali-Linux-8892eu.git

    you need to install unzip.
    
    sudo apt install unzip
    
    unzip rtl8192eu-linux-driver-realtek-4.4.x.zip

    cd rtl8192eu-linux-driver-realtek-4.4.x
  
    The Makefile is preconfigured to handle most x86/PC versions. However, if you are compiling for something other than an intel x86 architecture, you need to first select the platform 
    for the Raspberry Pi, you need to set the I386 to n and the ARM_RPI to y:

   
    CONFIG_PLATFORM_I386_PC = n
    CONFIG_PLATFORM_ARM_RPI = y

    For arm64 devices (e.g. Orange Pi PC 2):
   
    CONFIG_PLATFORM_I386_PC = n
    CONFIG_PLATFORM_ARM_AARCH64 = y

    Add the driver to DKMS. This will copy the source to a system directory so that it can used to rebuild the module on kernel upgrades.

    sudo dkms add .

    Build and install the driver.

    sudo dkms install rtl8192eu/1.0

    Distributions based on Debian & Ubuntu have RTL8XXXU driver present & running in kernelspace. To use our RTL8192EU driver, we need to blacklist RTL8XXXU.

    echo "blacklist rtl8xxxu" | sudo tee /etc/modprobe.d/rtl8xxxu.conf

    Force RTL8192EU Driver to be active from boot.

    echo -e "8192eu\n\nloop" | sudo tee /etc/modules

    Newer versions of Ubuntu has weird plugging/replugging issue (Check #94). This includes weird idling issues, To fix this:

    echo "options 8192eu rtw_power_mgnt=0 rtw_enusbss=0" | sudo tee /etc/modprobe.d/8192eu.conf

    Update changes to Grub & initramfs

    sudo update-grub; sudo update-initramfs -u

    Reboot system to load new changes from newly generated initramfs.

    systemctl reboot -i

    Check that your kernel has loaded the right module:

    sudo lshw -c network
    
    You should see the line driver=8192eu (if you are using linux on virtual box envoirment then you should see USB)



If you wish to uninstall the driver at a later point, use sudo dkms uninstall rtl8192eu/1.0. To completely remove the driver from DKMS use sudo dkms remove rtl8192eu/1.0 --all.
Using as AP

