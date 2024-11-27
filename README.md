# Wi-Fi adapters

## Uses and internal vs. external

Internal Wi-Fi adapters already exist in most modern laptops and desktop computers. They may also be referred to as ‘wi-fi cards’, especially when discussing the internal type. They allow a PC to transmit and receive wireless signals and data. Although this capability seems ubiquitous, it does not exist in some devices. They can also boost signal strength, increase effective range, and improve performance for existing internal adapters.  

The adapters we are discussing in this project are external USB pluggable devices. These are hardware devices that can provide wireless connectivity if the device does not already have internal Wi-Fi capabilities. However, depending on the specific chipset present in the USB adapter, it is also possible to use these devices for passive Wi-Fi monitoring and penetration testing.  

Some devices may work natively in certain environments (Windows, Kali) if supported. However, many chipsets may require specific drivers to function properly. We will see an example of this requirement. If the specific chipset is present, we can achieve monitor mode and packet injection.
Choosing ‘the right adapter’ is based on many factors and each has a mix of strengths and weaknesses, including:

- Frequency band capability (2.4 and/or 5 GHz)
- Size of device (larger usually means better antenna, reception)
- Speed ratings it can utilize for pentesting (802.11 a/b/g/n/ac/ax)
- Compatibility (native function, or need for drivers and advanced configuration)
- Security protocols supported (WPA, WPA2, WPA3)
- Monitor mode and packet injection capable (this is essential for the tasks we are discussing)

See this [resource](https://github.com/morrownr/USB-WiFi?tab=readme-ov-file) for more information, especially which devices work natively in Kali Linux without the need for additional drivers.

## ALFA AWUS 036ACS

This adapter has a compact form factor and is not very noticeable. It is not supported natively in Kali Linux and will require the installation of community supported drivers to give it full functionality. There are two main sources for drivers. ALFA does not provide very good driver support for many of its devices related to my specific purpose. GitHub has two main repositories for ALFA device drivers.

- The [aircrack-ng](https://aircrack-ng.org/) project page.
- GitHub user [morrownr](https://github.com/morrownr) is known as a valuable community source for drivers of these devices.

## Installing drivers
The following worked to install drivers for this device inside of a Kali virtual machine. It involves updating and upgrading our Kali installation, rebooting, installing headers for the current version, and installing dependent packages. Making a directory, cloning a repository, and running the install script to install the drivers.

Prior to performing these commands, we can see that there is no support present by connecting the USB device and typing:
- `lsusb`and pressing Enter
  - In the output I see no mention of the USB connected device by name

Open a new terminal window and use the following commands.
1.	sudo apt update && sudo apt upgrade
2.	sudo reboot
3.	sudo apt install -y linux-headers-$(uname -r) build-essential bc dkms git libelf-dev rfkill iw
4.	mkdir -p ~/src
5.	cd ~/src
6.	git clone https://github.com/morrownr/8821au-20210708.git
7.	cd ~/src/8821au-20210708
8.	sudo ./install-driver.sh

After the installation of the drivers, it is best to reboot the VM completely. After this I can test for recognition of the device by entering:

- `lsusb`
  - The output has changed, and I see the USB connected device listed by name and which drivers are installed.
- `iwconfig`
  - This shows that the device is in managed mode. Which is not suitable for my intended purpose. We can address this in the next commands.
    For now, we notice that the interface is named `wlan0`.

Now by entering:
- `sudo airmon-ng`
  - I can see as shown in the image below, the adapter has entered into `monitor mode`. This means that it is ready to passively monitor nearby wi-fi traffic.
 
Further discussion of wireless monitoring and attack methods won’t be discussed in this project. However, we now have a fully functional device for using Kali Linux wireless auditing tools like the `aircrack suite` and `wifite`.
