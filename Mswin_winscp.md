## How to download camera data using WinSCP

### WinSCP installation

You will need a secure file transfer software to download the pictures
into your laptop. One of the options is [WinSCP](http://winscp.net/),
[here](http://winscp.net/eng/download.php#download2) is a direct
download link.

*Note: When installing WinSCP, it may offer you Google Chrome
installation or perhaps something else as a bundle. You do not need
Google Chrome to communicate with the DFN camera. Skip the Google Chrome
installation by un-checking the "Include Google Chrome, along with the
WinSCP" unless you really want Chrome as well.*

Select “Explorer interface” when asked during the installation. (If you
miss this one, it can be changed later.) When the installation is
finished, WinSCP is launched automatically.

### Initial WinSCP configuration

Create a new connection – see screenshots below. The camera host IP
address in this example is 172.16.0.101 (local WiFi connection at the
camera site). Use the appropriate IP address corresponding to connection
used. (More on connecting to camera
[here](Getting_Started#Connecting_to_Your_Fireball_Observatory_for_the_First_Time.html).)

More detailed instructions how to use WinSCP are in pdf format here:
![<File:WinSCP> connection to
camera.pdf](WinSCP_connection_to_camera.pdf "File:WinSCP connection to camera.pdf"),
including screenshots.

### Setting up ssh key in WinSCP

If you use ssh terminal to connect to the camera systems, you probably
already have a ssh key pair generated and installed on your computer and
the target camera system(s). In that case, please check the [WinSCP
on-line
documentation](https://winscp.net/eng/docs/ui_login_authentication) how
to [tell WinSCP where your private ssh key file is stored on your
computer](https://winscp.net/eng/docs/ui_login_authentication#private_key).
Alternatively, you can use
[pageant](https://winscp.net/eng/docs/ui_pageant).

If you have never used ssh keys before, have a look at the [DFN wiki
here](SSH_Keys.html) and there is also a good [general information
on using ssh keys in
WinSCP](https://winscp.net/eng/docs/guide_public_key) on their web site.