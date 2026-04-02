## Advantech MIO 5250 single board industrial PC

This is the PC with a large black heatsink.

### BIOS update

You will need: PC (Advantech), VGA monitor, keyboard, mains power
supply, USB stick containing FreeDOS, patching utility and BIOS binary.

BIOS Version V1.13 to be installed. The BIOS binary image is in the
mercurial repo (dfn_repo/doc/resources/HW/PC/Advantech/MIO-5250/BIOS).

Steps:

\- Execute BIOS patching (Advantech: BIOS built-in patching function,
BIOS image ion USB stick)

\- Verify that the BIOS version changed to V1.13 CPU C-state report:
Disabled (after loading BIOS defaults)

### BIOS configuration

1\) Advanced -\> PPM configuration: CPU C-state report: Disabled

2\) Advanced -\> ACPI settings -\> Resume on RTC alarm: Enable, 8:00
(UTC)

3\) Advanced -\> IDE: AHCI mode

4\) Chipset -\> South bridge: Restore AC power loss: Power ON