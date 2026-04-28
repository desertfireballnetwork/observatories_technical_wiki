---
title: "Zbook Hibernate"
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

### 1.1 Swap settings
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

### 1.2 make sure secure boot in BIOS is disabled
Verify from linux console w/o reboot:
```
mokutil --sb-state
SecureBoot disabled
```
All good in this case - reboot to BIOS and change if not.

## 2. Set resume parameter for the grub bootloader
Point it to swap partition/file.
```
sudo emacs /etc/default/grub
...
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash resume=/dev/nvme0n1p3"
...

sudo update-grub
```

## 3. Enable Hibernate option in Power-Off Menu
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

## 4. Test 

Shutdown->hibernate menu item is there. Hibernate from menu works.

Commandline: (also from text console, eg Ctrl+Alt+F2)

```
sudo systemctl hibernate
```

Both works!

It takes quite long time though, several minutes, both the hibernating and restoring, so be patient. The hibernating turns screens off shortly after initiating, then it lights up  again soon, which is confusing. Particularly because the mouse/keyb control is seemingly "frozen" until the laptop actually fully hibernates.

_Note: Any interraction with the laptop during the process of hibernation (eg touching keyboard, shutting down the lid, unplugging/plugging anything to a USB-C including dock) is not recommended, as the process might get interrupted._

## 5. Celebrate! Play with extras... or troubleshoot

### 5.1 Extras

In Control center -> Power Management -> On Battery Power, for "When battery power is critically low", set **Hibernate**.

Then the system should hibernate to disk, minimising the power consumption, rather than shutdown or hard crash running out of battery capacity.

---

#### 5.1.1 Other useful tools, config and power related (optional)
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

### 5.2 Issues and troubleshooting

#### 5.2.1 External HDMI screen may be blank on resume
Fix: Switch that ext screen video output off in xrandr/display settings, then hit "restore previous configuration" to actually not switch it off.

Alternative fix: after full wake-up, disconnect and reconnect the dock or HDMI screen cable.

#### 5.2.2 Unwanted wake-ups

The Zbook wakes up from both hibernate and suspend when USB-C power adapter is unplugged.

Possible solutions:

**A1 Temporary solution**
```
sudo grep  "XHC" /proc/acpi/wakeup
XHCI      S0    *enabled   pci:0000:00:14.0
TXHC      S4    *enabled   pci:0000:00:0d.0
sudo echo "XHCI" | sudo tee /proc/acpi/wakeup
XHCI
sudo grep  "XHC" /proc/acpi/wakeup
XHCI      S0    *disabled  pci:0000:00:14.0
TXHC      S4    *enabled   pci:0000:00:0d.0
sudo echo "TXHC" | sudo tee /proc/acpi/wakeup
TXHC
martin@mcu-zbook:~$ sudo grep  "XHC" /proc/acpi/wakeup
XHCI      S0    *disabled  pci:0000:00:14.0
TXHC      S4    *disabled  pci:0000:00:0d.0
```
_Note:
`XHCI      S0` indicates S0 low power sub-mode (another poor engineering Microsoft enforced annoyance) is used instead of propper S3 mode._

This can also be verified by `cat /sys/power/mem_sleep`.

If you see `s2idle`: Your laptop is using "Modern Standby". Your XHCI device WILL wake the laptop from sleep because "Sleep" is technically an S0 state, not a propper S3 suspend to RAM.
If you see `s2idle [deep]`: Your laptop is using traditional S3 sleep. Your XHCI device SHOULD NOT wake the laptop from deep sleep.

My Zbook has 
```
cat /sys/power/mem_sleep
[s2idle]
```
which means laptop’s firmware (BIOS) has entirely disabled or removed support for traditional S3 "Deep Sleep"/"Suspend to RAM".
This is common on most laptops built since 2021 (Intel 11th Gen and newer). Microsof pushed everyone to implement "Modern Standby" (also called S0ix) and block users from using traditional S3.

**A1 Permanent - create script that toggles this**

Automated Sleep Script (For persistent resets)

Sometimes the system resets these "enabled" flags every time it wakes up. You can create a script that runs every time the system prepares to sleep.

Create a new script triggered by sleep (pre/post): `/lib/systemd/system-sleep/disable-unwanted-wakeup`

Paste the following content into a script 
```
#!/bin/sh

safe_disable() {
    # 
    # write to /proc/acpi/wakeup is a toggle - not set/unset
    if grep -iwq "$1.*enabled" /proc/acpi/wakeup; then
        echo "$1" > /proc/acpi/wakeup
        logger -t "$0" -p info "My wakeup tweak: Disabling ACPI event $1 to wake up from sleep."
    else
        logger -t "$0" -p info "My wakeup tweak: ACPI event $1 wake-up was already disabled."
    fi
}

logger -t "$0" -p info "My wakeup tweak: Running $0 $*."

case $1 in
  pre)
    safe_disable "TXHC"
    safe_disable "XHCI"
    ;;
esac
```
Make the script executable: sudo chmod +x /lib/systemd/system-sleep/disable-unwanted-wakeup

**A2 permanent - config file**
Update your configuration to include all three "power-aware" triggers: AWAC (Alarm), TXHC (Thunderbolt/USB-C), and XHCI (Standard USB).
```
sudo emacs /etc/tmpfiles.d/disable-wakeups.conf
```
Paste these three lines:
```
# Path                  Mode UID  GID  Age Argument
w /proc/acpi/wakeup     -    -    -    -   AWAC
w /proc/acpi/wakeup     -    -    -    -   TXHC
w /proc/acpi/wakeup     -    -    -    -   XHCI
```
Save and exit.

Other possible culprits to add are these Thunderbolt wake-ups

```
### Thunderbolt PCIe root ports
w /proc/acpi/wakeup     -    -    -    -   TRP0
w /proc/acpi/wakeup     -    -    -    -   TRP2
### Thunderbolt NHI (Native Host Interface) controllers
w /proc/acpi/wakeup     -    -    -    -   TDM0
w /proc/acpi/wakeup     -    -    -    -   TDM1
```
Pretty much anything that is enabled when listing with 

```
grep  "enabled" /proc/acpi/wakeup
```

## 6. Resources and other reads

Comprehensive generic instructions for suspend in all recent Ubuntu versions, updated as new versions are released
https://ubuntuhandbook.org/index.php/2021/08/enable-hibernate-ubuntu-21-10/
