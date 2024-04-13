# rpi_gadget_mode
Instructions for enabling **USB (ethernet) gadget mode** on **RPi 4 and RPi Zero 2W**. All you need is a USB 2.0 Type C cable for RPi 4, or a USB 2.0 micro usb for RPi Zero 2W. OTG cable **NOT** needed.

These steps allow for both **normal** operation and **USB gadget mode**. For **home use** gadget mode is effectively **worthless**, as the RPi can operate **headless** and can be connected to using VNC or XRDP. **At work** gadget mode is **useful** as BYOD (Bring Your Own Device) devices are not allowed to connect to the corporate network. My work monitor/keyboard/mouse needs to be disconnected from my company laptop and then connected to the RPi (a pain in the ass). In gadget mode the RPi will still use the BYOD wifi for internet access, but the **remote desktop connection is over USB cable**. No more switching the monitor/keyboard/mouse connections (hoorah!)

**Steps:**

(1) Become superuser (sudo -i) and do all the following steps...

(2) Make backup copies of /boot/config.txt and /boot/cmdline.txt

(3) Edit **/boot/config.txt** by appending the following line to the end of the file:

``` dtoverlay=dwc2 ```

(4) Edit **/boot/cmdline.txt** by inserting the following text between **rootwait** and **quiet**

``` modules-load=dwc2,g_ether g_ether.dev_addr=12:34:56:78:9a:bc g_ether.host_addr=16:23:45:78:9a:bc ```

(5) Create a new udev rule file. Create **/etc/udev/rules.d/90-usb-gadget.rules** and insert the following:

``` SUBSYSTEM=="net",ACTION=="add",KERNEL=="usb0",RUN+="/sbin/ifconfig usb0 192.168.1.2 netmask 255.255.255.0",RUN+="/usr/bin/python -c 'import time; time.sleep(20)'",RUN+="/sbin/ip route add 192.168.1.1 dev usb0" ```

(6) Make a backup copy of /etc/sysctl.conf

(7) Edit **/etc/sysctl.conf** and append the following line to the end of the file (this allows **normal mode**, when not a gadget):

``` net.ipv4.conf.all.ignore_routes_with_linkdown=1 ```

That's it.

Here are the **steps** I needed to follow on [Ubuntu](https://github.com/charkster/rpi_gadget_mode/blob/main/ubuntu_install_instructions.md)
, [Windows 10](https://github.com/charkster/rpi_gadget_mode/blob/main/windows_10_install_instructions.md) and [MacOS](https://github.com/charkster/rpi_gadget_mode/blob/main/images/macos_rpi_gadget_config.png)

I used this [forum_post](https://forums.raspberrypi.com/viewtopic.php?t=306121&sid=6f23dece3a28a0281b971be8b0ec9763&start=25) as my starting point for the above instructions (thank you!)

The Windows driver mod-duo-rndis.zip (INF and CAT files) were found on Github here: https://github.com/dukelec/mbrush/tree/master/doc/win_driver

# Additional Steps for XRDP using pi user
```
sudo apt-get install xrdp
sudo adduser xrdp ssl-cert
```
```
sudo vi /etc/X11/xrdp/xorg.conf
```

Find :

``` Option "DRMDevice" "/dev/dri/renderD128" ```

Replace with:
```
#Option "DRMDevice" "/dev/dri/renderD128"
Option "DRMDevice" ""
```

**That's it.**

# Additional Steps for XRDP on RPi Zero 2W

On your PI, create a file /usr/bin/startlxde-pi-xrdp with the following contents:-

```
#!/bin/sh
# See https://github.com/neutrinolabs/xrdp/discussions/2050

if [ -n "$XRDP_SESSION" ]; then
    # Override vcgencmd to return no memory. This causes the
    # openbox window manager to be used instead of mutter
    vcgencmd()
    {
        if [ $1/$2 = get_config/total_mem ]; then
            echo total_mem=0
        else
            echo "** Fix stub vcgencmd for $*" >&2
            false
        fi
    }
fi

. /usr/bin/startlxde-pi
```

Make the script executable with ``` sudo chmod +x /usr/bin/startlxde-pi-xrdp. ```

If you've only got one user on your PI, you can configure the PI to use this script for X sessions with this comand:-
```
ln -sf /usr/bin/startlxde-pi-xrdp ~/.xsession
```
**Remeber to increase your swap size:** ```sudo vi /etc/dphys-swapfile```

Make these changes (make sure they are not commented-out)"
```
CONF_SWAPSIZE=1024
CONF_MAXSWAP=2048
```
# Additional Steps for REALVNC server compatibility with Remmina

Edit **/root/.vnc/config.d/vncserver-x11** and add:
```
Encryption=PreferOn
Authentication=VncAuth
```
**Run this to set the VncAuth password:**
```
sudo vncpasswd -legacy -service
```
