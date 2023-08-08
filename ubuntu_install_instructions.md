**Instructions for configuring USB (ethernet) gadget in Ubuntu.**

(1) dmesg should show the new usb device, properly identified as a RNDIS/Ethernet Gadget.

![picture](https://github.com/charkster/rpi_gadget_mode/blob/main/images/ubuntu_dmesg.png)

(2) In Settings -> Network the new device should be listed as USB Ethernet:

![picture](https://github.com/charkster/rpi_gadget_mode/blob/main/images/ubuntu_network_devices.png)

(3) Configure with the following IPv4 settings:

![picture](https://github.com/charkster/rpi_gadget_mode/blob/main/images/ubuntu_usb0_ipv4_settings.png)

(4) The summary should look like this:

![picture](https://github.com/charkster/rpi_gadget_mode/blob/main/images/ubuntu_usb0_network_properties.png)

(5) The RPi address will be **192.168.1.2**, and the interface to it is 192.168.1.1
