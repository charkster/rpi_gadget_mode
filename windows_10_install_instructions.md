**Instructions for configuring USB (ethernet) gadget in Windows 10.**

(1) Most times the usb gadget is identified as a USB com port:

![picture](https://github.com/charkster/rpi_gadget_mode/blob/main/images/windows_raspberry_pi_gadget_mode_1.png)

(2) Download  [mod-duo-rndis.zip](https://github.com/charkster/rpi_gadget_mode/blob/main/mod-duo-rndis.zip) and unzip

(3) Update the driver

![picture](https://github.com/charkster/rpi_gadget_mode/blob/main/images/windows_raspberry_pi_gadget_mode_2.png)

(4) Browse my computer for drivers (select the directory where mod-duo-rndis.zip was extracted)

![picture](https://github.com/charkster/rpi_gadget_mode/blob/main/images/windows_raspberry_pi_gadget_mode_3.png)

(5) Correct driver should be seen now

![picture](https://github.com/charkster/rpi_gadget_mode/blob/main/images/windows_raspberry_pi_gadget_mode_4.png)

(6) In Settings -> Networking -> Ethernet, a new ethernet device should be visible

![picture](https://github.com/charkster/rpi_gadget_mode/blob/main/images/windows_raspberry_pi_gadget_mode_5.png)

(7) Configure the IPv4 settings as shown

![picture](https://github.com/charkster/rpi_gadget_mode/blob/main/images/windows_raspberry_pi_gadget_mode_6.png)

(8) The RPi address will be **192.168.1.2**, and the interface to it is 192.168.1.1
