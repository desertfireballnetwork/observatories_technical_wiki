---
title: "Zbook Sleep and Hibernate"
has_children: true
parent: "Zbook"
---

# Instructions to enable S4 hibernate on 2025 ZBook

This mode is also called "suspend to disk". It is supposed to have the lowest power consumption, pretty much same as S5 power down.

This how-to was inspired by generic Ubuntu suspend instructions - see resources in the bottom of the page. That includes details and other scenarios.

_(Tested on: SBKPFV3 HP ZBook 8 G1i 14 inch Mobile Workstation PC)_

## 1. Make sure there is swap partition or swap file
Ideally bigger than physical RAM. That is where the memory state will be saved as the RAM is not powered in S4 hiberbate.

```
swapon --show
NAME           TYPE       SIZE USED PRIO
/dev/nvme0n1p3 partition 74.5G   0B   -2
```
If not, create swap partition or swap file - see Step 1 of the instructions (link at the bottom of this page)

### 1.1) Swap settings
I have plenty of RAM and I like to reduce swapping to only necessary situations, so that there is ideally "_always_" enough space on swap for suspend to disk:
```
sudo su -c "echo "1" > /proc/sys/vm/swappiness"
cat /proc/sys/vm/swappiness  
1
```
Make it permanent.
```
sudo emacs /etc/sysctl.conf
vm.swappiness=1
```

### 1.2) make sure secure boot in BIOS is disabled
Verify from linux console w/o reboot:
```
mokutil --sb-state
SecureBoot disabled
```
All good in this case - reboot to BIOS and change if not.

## 2) Set resume parameter for the grub bootloader
Point it to swap partition/file.
```
sudo emacs /etc/default/grub
...
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash resume=/dev/nvme0n1p3"
...

sudo update-grub
```

## 3) Enable Hibernate option in Power-Off Menu
```
sudo apt install polkitd-pkla
sudo mkdir -p /etc/polkit-1/localauthority/50-local.d
```
Create file `/etc/polkit-1/localauthority/50-local.d/com.ubuntu.enable-hibernate.pkla`
with the following content:
```
[Re-enable hibernate by default in upower]
Identity=unix-user:*
Action=org.freedesktop.upower.hibernate
ResultActive=yes

[Re-enable hibernate by default in logind]
Identity=unix-user:*
Action=org.freedesktop.login1.hibernate;org.freedesktop.login1.handle-hibernate-key;org.freedesktop.login1;org.freedesktop.login1.hibernate-multiple-sessions;org.freedesktop.login1.hibernate-ignore-inhibit
ResultActive=yes
```

or

create file `/etc/polkit-1/rules.d/10-enable-hibernate.rules`
with the following content:

```
polkit.addRule(function(action, subject) {
   if (action.id == "org.freedesktop.login1.hibernate" ||
       action.id == "org.freedesktop.login1.hibernate-multiple-sessions" ||
       action.id == "org.freedesktop.upower.hibernate" ||
       action.id == "org.freedesktop.login1.handle-hibernate-key" ||
       action.id == "org.freedesktop.login1.hibernate-ignore-inhibit")
   {
       return polkit.Result.YES;
   }
});
```

_(I did both, my desktop manager is Mate.)_

Reboot.

## 4) Test: 

Shutdown->hibernate menu item is there. Hibernate from menu works.

Commandline: (also from text console, eg Ctrl+Alt+F2)

```
sudo systemctl hibernate
```

Both works!

It takes quite long time though, several minutes, both the hibernating and restoring, so be patient. The hibernating turns screens off shortly after initiating, then it lights up  again soon, which is confusing. Particularly because the mouse/keyb control is seemingly "frozen" until the laptop actually fully hibernates.

_Note: Any interraction with the laptop during the process of hibernation (eg touching keyboard, shutting down the lid, unplugging/plugging anything to a USB-C including dock) is not recommended, as the process might get interrupted._

## 5) Celebrate! Play with extras... or troubleshoot

### 5.1) Extras

In Control center -> Power Management -> On Battery Power, for "When battery power is critically low", set **Hibernate**.

Then the system should hibernate to disk, minimising the power consumption, rather than shutdown or hard crash running out of battery capacity.

---

#### Other useful tools, config and power related (optional)

dconf-editor

```
sudo apt install dconf-editor
sudo dconf-editor
```

TLP

```
sudo tlp star

sudo tlp-stat -s
sudo tlp-stat
```
config: /etc/tlp.conf

(https://www.fosslinux.com/135830/how-to-fine-tune-power-management-in-ubuntu-using-tlp.htm)

### 5.2) Issues 

#### External HDMI screen may be blank on resume
Fix: Switch that ext screen video output off in xrandr/display settings, then hit "restore previous configuration" to actually not switch it off.

Alternative fix: after full wake-up, disconnect and reconnect the dock or HDMI screen cable.

#### 

## 6. Resources

### Comprehensive generic instructions for suspend in all recent Ubuntu versions.
https://ubuntuhandbook.org/index.php/2021/08/enable-hibernate-ubuntu-21-10/
