SSH is a secure way of logging into a terminal session on a remote
computer. SSH keys are a way of authenticating with an SSH server that
can be used instead of passwords. They are faster and more convenient
than password based authentication and can be more secure. They are the
recommended authentication method to use when logging into DFN
observatories.

The two biggest advantages of keys over passwords are–

- that you can unlock your personal key once (per local session) and
  then log into as many different ssh-servers (cameras in this case) as
  you want without having to enter your password again using an
  [ssh-agent](wikipedia:Ssh-agent "wikilink") and
- that you can then disable password based login to protect against
  brute force dictionary attacks.

Once you have one, SSH keys are much faster and easier to use than
passwords, but creating one can be a bit daunting.

# Creating your SSH key

The SSH key that you create will consist of two parts: a public key and
a private key. The private key is your authentication or identity
document and you must keep it secure. This is done by password
protecting it when you create it and also restricting access to the file
(you shouldn't give your private key to other people). The public key is
used to check that the person logging in possesses the private key.
(More on asymmetric cryptography here:
<https://en.wikipedia.org/wiki/Public-key_cryptography>.) These
instructions will help you create an ssh key on Linux, Mac OS X or
Windows using the [Microsoft Windows Linux
Subsystem](Logging_in_locally_via_WiFi_or_Ethernet#Logging_in_from_Windows "wikilink").

First, check to see if you have any ssh keys already by running the
command:

`ls ~/.ssh/id_*`

If the directory doesn't exist, or no matching files are found, then you
do not have existing key and you can proceed.

If you do have an existing key, **and it is password protected**, you
may want to use it (skip to [Installing your SSH
key](#Installing_Your_SSH_Key "wikilink")), or you may want to generate
a new key just for your observatories alongside your old key (in this
case please backup your existing and adapt the below instructions to
avoid overwriting your existing key).

Create your SSH key with the command: (substituting your email address
for `your_email@address.com` or some other identifying information such
as `name@institution`)

`ssh-keygen -t ed25519 -C "your_email@address.com"`

or

`ssh-keygen -t rsa -b 4096 -C "your_email@address.com"`

After running this command you will be prompted for the key location.
Press enter to accept the default location for saving the key.
ssh-keygen will now prompt you to enter a password. **It is very
important that you enter a password at this stage.**

**<big>Never install a key that is not password protected on a DFN
observatory or computer.</big>**

Press enter once you have entered your password and you will be prompted
to confirm your password by entering it again (press enter again
afterwards). Your key has now been generated and is ready to install.

The private key is now located at `~/.ssh/id_rsa` and your public key is
at `~/.ssh/id_rsa.pub`. At this stage you probably want to back both of
these files up in a safe and secure place (although the public key can
be regenerated later from the private key if required).

# Installing Your SSH Key

Now it is time to install your SSH key onto the computer or DFN
observatory that you want to log in to. If you have access to the system
you would like to install your key on, this is easily accomplished with
the `ssh-copy-id` command. This command will connect to an SSH server,
prompt you for the password and then install your public key for you.
You need to know the username that you want to log into (probably `root`
if you're configuring or doing maintenance on a DFN observatory), the
user's password, and the [IP address of the
observatory](IP_Addresses "wikilink") or computer.

As an example, to install your key into DFNSMALL17 for the root user you
would run one of the following commands:

*Note: this direct installation of public ssh key will not work with
user root on DFNEXT camera systems, when installing your first key and
authenticating using system password. See [here for explanation
why](Logging_in_locally_via_WiFi_or_Ethernet#Difficulties_logging_in_as_the_root_user_using_a_password "wikilink").
One needs to log in as dfn-user, then 'su -' to become root and [install
the public key manually](#Installing_your_SSH_key_manually "wikilink").*

#### If connecting remotely over the [VPN](VPN "wikilink"):

`ssh-copy-id root@10.1.17.17 `

#### If connecting over the VPN with a previously configured hosts file on your local machine (to make things easier):

`ssh-copy-id root@DFNSMALL17 `

#### If connecting over Ethernet (Laptop/eth0 port):

`ssh-copy-id root@172.16.1.101 `

#### If connecting over WiFi (observatory in AP mode):

`ssh-copy-id root@172.16.0.101 `

After running this command, you will be prompted for the remote user's
password (for example the `root` user's password on `DFNEXT017`). Enter
the password and press enter. Your key should now be installed. You can
test this by SSHing into the remote system, you should be prompted for
the password do your key instead of the password of the remote user.

For example: `ssh root@DFNSMALL17`

## Installing your SSH key manually

It is also possible to install your SSH key manually. To do this, log
into the system you wish to use your key with and copy your public key
(inside `id_rsa.pub` which you can open with a text editor) and paste it
into the `authorized_keys` file of the user you would like to log in as.

The root user's `authorized_keys` file is located at
`/root/.ssh/authorized_keys`, and, as an example, the authorized_keys
file for a user named `robin` would be
`/home/robin/.ssh/authorized_keys`.

Or, alternatively, transfer the public key to the target system and
append it to the authorized_keys list (WARNING: make sure you will not
overwrite/remove/mis-edit the existing keys there!)

Example (for rsa public key; for the more modern and faster ed25519, the
default public key file name is id_ed25519.pub)

1\) From your linux/mac system:

`scp id_rsa.pub dfn-user@172.16.0.101:/home/dfn-user/my_id_rsa.pub`

2\) On the observatory system (make sure to use double '\>'!) :

`cat /home/dfn-user/my_id_rsa.pub >> ~/.ssh/authorized_keys`

*Note: allways add your key at the end of the authorized_keys file.*

*Note: on Windows, the generated public key may have slightly different
format than what authorized_keys file requires; in such a case you may
need to edit it on the target system.*

## Getting somebody else to install your key

If you are creating your key to get access to your observatory for the
first time and password based authentication is turned off (or you have
not been provided with the password) you will need to get someone to
install the key for you.

They will need your **public** key for this. **Nobody will ever ask you
for your private key; never give someone (even the DFN) your private
key.** You can send the person installing your key either the
`id_rsa.pub` file or the text inside this file. Make sure that the end
of this line identifies you in some way, either with
`your_email@address.com`, `your_name@institution` (such as
`robert@curtin`) or something similar. If this is missing or incorrect
you can edit it without affecting the integrity of your key. As the
public key only identifies the private key and does not let you recover
the private key, it is usually OK to send the public key over insecure
channels such as email (although you would not want the key to be
covertly swapped with someone else's as then they would be able to log
in instead of you).

To get access to your DFN observatory, send your public key to [DFN
camera help](Getting_Help "wikilink") along with the name of the
observatory or observatories you would like to login to (e.g.
`DFNEXT032`), and plug the relevant observatories into power and
internet (use the *Network/eth1* Ethernet port). We will then install
your keys and email you back to let you know that you can now log in
easily.

# Running an SSH Agent

The real benefit of using an SSH key is that you don't have to enter
remote user passwords every time you log into a new remote system, but
they way things are configured now, you will have to unlock your key
with a password every time you want to start a new SSH session. While
this is helpful as you only have to remember one password for your local
key now instead of multiple passwords for multiple remote users, SSH
keys really become user friendly once we start an SSH agent.

An SSH agent prompts you for the password to unlock your key and then
keeps the unlocked key available in memory (RAM) so that you don't need
to keep unlocking your key. You can continue to use this unlocked
version without entering your password again until you terminate your
local session (by logging off of or powering down your local computer).

Many modern GNU/Linux desktop environments (such as Gnome, Cinnamon and
Unity) will automatically start an SSH agent the first time you use your
SSH key after logging in, but if you need to start one manually you can
also do that.

Open a terminal window and run the command:

`ssh-agent`

The output will look something like this:

`SSH_AUTH_SOCK=/tmp/ssh-DODfkAlnVz9s/agent.22714; export SSH_AUTH_SOCK;`
`SSH_AGENT_PID=22715; export SSH_AGENT_PID;`
`echo Agent pid 22715;`

Copy the output, paste it back into the terminal and press enter to
connect up your SSH agent.

## Starting an SSH Agent Automatically with the Microsoft Windows Subsystem for Linux

After your keys are copied into `~/.ssh`, you can put [this
snippet](https://gist.github.com/copperlight/e9eaa40d6f3bb9e1e6e6b1dd8ba0a8d5)
into your `.bashrc` file to start the SSH agent automatically when you
open up your bash terminal if you're using the Microsoft Windows
Subsystem for Linux.

# Windows Users Not Using the Microsoft Windows Subsystem for Linux

![](PuTTYgen_Example_Screenshot.png "PuTTYgen_Example_Screenshot.png")
If you're using Windows, but are not using the [Microsoft Windows
Subsystem for
Linux￼](Microsoft_Windows_Subsystem_for_Linux￼ "wikilink"). You will
need to use PuTTYgen ([which come as part of PuTTY if you use the
installer](Logging_in_locally_via_WiFi_or_Ethernet#PuTTY "wikilink")) to
generate your key. To generate a key using PuTTYgen: run PuTTYgen from
the start menu; select SSH-2 RSA down the bottom of the window; change
the number of bits to 4096; and hit *Generate*. Or use more modern and
faster ed25519 key.

You will then be asked to create some randomness by moving your mouse in
order to generate the key. Once this is complete **we recommend to enter
and confirm a password to protect your private key.** That is for the
case somebody broke into your Windows computer - in case of unprotected
private key, they would get access to all the cameras where your public
key is installed.

After you have entered a password, save your private key by pressing
*Save private key* and picking a location. We suggest naming the private
key something like `private_key.ppk` or something similar. (You might
want to add something to identify it if you have multiple keys. However,
you will need to use the `.ppk` extension.

The public key is als included in the .ppk file, and it can also be
regenerated in the future so saving the public key is optional, but you
will probably want to do it anyway. Press *Save public key* and pick a
location to save your public key. We suggest naming the file
`public_key.txt` or something similar (you don't want to confuse it with
the private key). If you're going to be installing the public key (use
the manual instructions above) or sending it to someone to install for
you, you might want to take this opportunity to copy the public key from
the window at this stage.

## Running the Pageant SSH Agent

The SSH agent used with PuTTY is called Pagent and will have been
installed with PuTTY if you used the installer (instead of just
downloading the PuTTY binary). You can run it from the start menu (it
will appear in you system tray area of your taskbar) and then load keys
by browsing to the relevant `.ppk` files, but if you usually use the
same key or keys, you might want to edit the start menu shortcut so that
it unlocks your key by default.

To do this, right click on the Pageant entry in the start menu, and
click *Properties* Then, find the field labelled *Target* and add the
path to your key in quotes after the path to the Pageant binary. Your
*Target* field should be changed from something like this:

`"C:\Program Files (x86)\PuTTY\pageant.exe"`

to something like this: (substituting the path to your key for
`C:\Path\To\Your\Key\id_rsa.ppk`)

`"C:\Program Files (x86)\PuTTY\pageant.exe" "C:\Path\To\Your\Key\id_rsa.ppk"`

You can now load your key without browsing to it when you launch
Pageant. It is possible to add multiple keys in this way if required.