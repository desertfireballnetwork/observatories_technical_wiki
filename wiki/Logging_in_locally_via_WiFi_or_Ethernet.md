This is the recommended way to log into your camera initially as it is
very similar to the [remote log
in](Logging_in_remotely_over_the_network "wikilink") method that you
will use later once your camera is installed. If you have trouble
logging in via Ethernet or WiFi, it is also possible to log in using an
HDMI or VGA monitor and a USB keyboard.

## Ethernet

All DFN observatories have two Ethernet ports. One is used to connect
the camera to the internet if there is wired internet available. (This
port is labled *Network/eth1* on the DFNEXT observatories.) The other
port is used for logging into the camera with your computer. (This port
is labelled *Laptop/eth0* on the DFNEXT observatory.) On DFNSMALL and
DFNKIT observatories the Ethernet port for local login should be closest
to the USB connectors. This is the port that will be used for these
instructions.

## WiFi

All DFNEXT systems have WiFi connectivity out of the box, and it is
possible to add WiFi to DFNSMALLs and DFNKITs as long as the correct USB
WiFi adapter is used. The observatory acts as a WiFi access point (this
is called AP mode); to connect, just select the relevant WiFi network
SSID from your observatory (e.g. `DFNEXT013` is the SSID of the WiFi
network to connect to observarory with hostname DFNEXT013).

Please contact the GFO team at Curtin (dfn.camera.help@gmail.com) for
the default WiFi password.

*Note: It is possible to find out and change the password in a
configuration file, please refer to [WiFi
configuration](WiFi_configuration "wikilink").*

One important thing to note is that, if the observatory is connected to
the internet via an existing local WiFi network it will be in client
mode instead of host mode, so there will be no DFN... network to connect
to. In this case you will need to connect to the same WiFi network that
the camera is using. The process of finding the IP address to log into
in this case is slightly different and depends on how the local WiFi
network is configured; it might be simpler to [log in via the
VPN](Logging_in_remotely_over_the_network "wikilink").

## Logging In via the Terminal

Whether you are connected to the camera via wired Ethernet or WiFi
(where the camera is in AP mode) the camera will provide you with an IP
address via
[DHCP](wikipedia:Dynamic_Host_Configuration_Protocol "wikilink") and
provide you with internet access if it is connected to the internet
(probably via mobile broadband or its *Network/eth1* Ethernet port).

To log in via the command line you will need to know the IP address of
the camera and the username you would like to use. As some of the
commands that we often use require root permissions, we usually log in
as the `root` user. The commands below should work for Linux and Mac OS
X users.

#### **If you are connected via Ethernet** (*Laptop/eth0* port)

the IP address of the observatory is `172.16.1.101` . You can connect by
opening a terminal and running the command:

`ssh root@172.16.1.101`

#### **If you are connected via WiFi** (observatory in AP mode)

the IP address of the observatory is `172.16.0.101` . You can connect by
opening a terminal and running the command:

`ssh root@172.16.0.101`

The camera will then prompt you for it's root password or you will
automatically logged in via your [ssh key](SSH_Keys "wikilink"). (SSH
keys are a more secure, faster and more convenient alternative to
passwords for logging into computers over the network).

#### Difficulties logging in as the root user using a password

For security reasons, password based authentication is disabled for the
root user by default in the latest ([DFNEXT](DFNEXT "wikilink"))
observatories. If you want to log in using passwords, you will need to
log in to the `dfn-user` user and then change to the root user using the
command:

`su -`

We suggest [installing your ssh key](SSH_Keys "wikilink") so that this
is not required every time you log in.

## Logging in via the Terminal from Windows

If you are using Windows, you will need to install an ssh program of
some kind. For Windows 7 and lower we recommend the Windows program
[PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
and **for Windows 10 we recommend using the [Microsoft Windows Subsystem
for Linux](https://msdn.microsoft.com/en-au/commandline/wsl/about)**
(through PuTTY still can be used).

### Microsoft Windows Subsystem for Linux

The advantages of using the Microsoft Windows Subsystem for Linux are
that the usage will more exactly match the instructions given here, and
you will have local access to the same command line tools that you will
use on the camera (which may be helpful). If you have very little drive
space available you may want to use PuTTY instead, as the Microsoft
Windows Subsystem for Linux installs a full command line Linux
compatibility layer (like having a virtual GNU/Linux distro installed
inside Windows, except all of the system calls are handled by the
Windows kernel instead of the Linux kernel) and this does take up some
storage space. To get similar functionality on older versions of
Windows, users can try [Cygwin](https://cygwin.com/install.html) or
install a Linux distribution such as [Debian](https://www.debian.org/)
as a [virtual machine](https://www.virtualbox.org/wiki/Downloads). After
installing the Microsoft Windows Subsystem for Linux, launch it from the
start menu and follow the above instructions as normal (as if you are on
Linux).
![](Putty_config_for_local_login.png "Putty_config_for_local_login.png")

### PuTTY

Installation using the MSI 'Windows Installer' is probably easiest. This
version is either the first of second file on the [PuTTY download
page](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).
Once installed, run PuTTY from the start menu, enter the IP address of
the camera in the configuration screen and hit "Open".

# Logging in via the [Web Interface](Web_Interface "wikilink")

Instead of using the text based terminal you can also use the [graphical
web interface](Web_Interface "wikilink") to control most functionality
of the camera. It is especially useful when using a mobile device.

To log into the web interface launch a web browser on your local
computer or mobile device and enter the IP address of the camera as well
as the web interface port (`8080`).

Please find more details on the [graphical web
interface](Web_Interface "wikilink") connection and usage
[here](Web_Interface "wikilink").

## Troubleshooting

Please refer to the [troubleshooting
page](Camera_Troubleshooting#How_to_troubleshoot_PC_booting "wikilink").