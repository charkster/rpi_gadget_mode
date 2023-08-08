# rpi_gadget_mode
Instructions for enabling **USB (ethernet) gadget mode** on **RPi 4 and RPi Zero 2W**. All you need is a USB 2.0 Type C cable for RPi 4, or a USB 2.0 micro usb for RPi Zero 2W. OTG cable **NOT** needed.

These steps allow for both **normal** operation and **USB gadget mode**. For **home use** gadget mode is effectively **worthless**, as the RPi can operate **headless** and can be connected to using VNC or XRDP. **At work** gadget mode is **useful** as BYOD (Bring Your Own Device) devices are not allowed to connect to the corporate network. My work monitor/keyboard/mouse needs to be disconnected from my company laptop and then connected to the RPi (a pain in the ass). In gadget mode the RPi will still use the BYOD wifi for internet access, but the **remote desktop connection is over USB cable**. No more switching the monitor/keyboard/mouse connections (hoorah!)

**Steps:**

(1) Become superuser (sudo -i) do all the following steps...

(2) Make backup copies of /boot/config.txt and /boot/cmdline.txt

(3) Edit **/boot/config.txt** by appending the following line to the end of the file:

**dtoverlay=dwc2**

(4) Edit **/boot/config.txt** by inserting the following text between **rootwait** and **quiet**

**modules-load=dwc2,g_ether g_ether.dev_addr=12:34:56:78:9a:bc g_ether.host_addr=16:23:45:78:9a:bc**

(5) Create a new udev rule file. Create **/etc/udev/rules.d/90-usb-gadget.rules** and insert the following:

**SUBSYSTEM=="net",ACTION=="add",KERNEL=="usb0",RUN+="/sbin/ifconfig usb0 192.168.1.2 netmask 255.255.255.0",RUN+="/usr/bin/python -c 'import time; time.sleep(20)'",RUN+="/sbin/ip route add default via 192.168.1.1 dev usb0"**

(6) Make a backup copy of /etc/sysctl.conf

(7) Edit **/etc/sysctl.conf** and append the following line to the end of the file (this allows **normal mode**, when not a gadget):

**net.ipv4.conf.all.ignore_routes_with_linkdown=1**

That's it.

Here are the **steps** I needed to follow on [Ubuntu](https://github.com/charkster/rpi_gadget_mode/blob/main/ubuntu_install_instructions.md)
and [Windows 10](https://github.com/charkster/rpi_gadget_mode/blob/main/windows_install_instructions.md)

I used this [forum_post](https://forums.raspberrypi.com/viewtopic.php?t=306121&sid=6f23dece3a28a0281b971be8b0ec9763&start=25) as my starting point for the above instructions (thank you!)

The Windows driver mod-duo-rndis.zip (INF and CAT files) were found on Github here: https://github.com/dukelec/mbrush/tree/master/doc/win_driver
