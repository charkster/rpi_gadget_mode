# rpi_gadget_mode
Instructions for enabling USB (ethernet) gadget mode on RPi 4 and RPi Zero 2W.

These steps allow for normal operation and USB gadget mode. For home use gadget mode is effectively worthless, as I can run headless and connect using VNC or XRDP. At work gadget mode is usefull as BYOD (Bring Your Own Device) devices are not allowed to connect to the corporate network. If I want to use my work monitor, keyboard and mouse with the RPi I would need to disconnect them from my company device and connect them to the RPi (this would be an addition cable connection to the monitor and moving the wireless keyboard/mouse dongle to the RPi). In gadget mode I still have the RPi still use the BYOD wifi, but my remote connection is over USB cable. No more switching the monitor/keyboard/mouse.

Steps:
(1) As superuser (sudo -i) do the following
(2) Make backup copies of /boot/config.txt and /boot/cmdline.txt
(3) Edit /boot/config.txt by adding the following line to the end of the file:
dtoverlay=dwc2
(4) Edit /boot/config.txt by adding the following text between "rootwait" and "quiet"
modules-load=dwc2,g_ether g_ether.dev_addr=12:34:56:78:9a:bc g_ether.host_addr=16:23:45:78:9a:bc
(5) Create a new udev rule file. Create /etc/udev/rules.d/90-usb-gadget.rules and insert the following:
SUBSYSTEM=="net",ACTION=="add",KERNEL=="usb0",RUN+="/sbin/ifconfig usb0 192.168.1.2 netmask 255.255.255.0",RUN+="/usr/bin/python -c 'import time; time.sleep(20)'",RUN+="/sbin/ip route add default via 192.168.1.1 dev usb0"
(6) Make a backup copy of /etc/sysctl.conf
(7) Edit /etc/sysctl.conf and add the following line to the end of the file:
net.ipv4.conf.all.ignore_routes_with_linkdown=1

That's it.
