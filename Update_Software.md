### Automated software updates

As long as the observatory is connected to the Internet, the software
that controls observatory auto updates daily from a dedicated DFN
server. There are two attempts to do so in the afternoon local time, ~
40 minutes before the daily reboot and ~ 20 minutes after; the default
times are 3:30 PM and 4:30 PM for the SW update and 4:10 PM for reboot.

### Manual software update over Internet

It might be handy to execute the network SW update manually, for example
in case of testing or deploying new observatory that was off-line for
some time or in transport or stored as a spare. In this case log in to
the observatory and execute command

`$ dfn_down_install_sw_from_server.sh `

### Manual software update using local copy

In case of remote site without Internet connection the only way to
update the observatory software is to bring a copy of the software eg on
laptop do the update locally.

The Australian DFN team members can find the latest stable software in
the internal DFN repo in operation/SW/dfnsmall/stable (DFNSAMLL type of
observatory) or operation/SW/dfnext/stable (DFNEXT type). External
collaborators will be provided with a copy of the software on request.

Assuming the servicing person has Linux environment on her/his laptop,
first step is to do a dry-run to check there are no command typo errors
by running:

`$ rsync -nrv opt usr root@172.16.1.101:/`

If there are no errors or anomalies, just list of files that would copy,
you can run the real update:

`$ rsync -rv opt usr root@172.16.1.101:/`

*Note: The above IP address is for local wired connection (over
etherenet cable), for WiFi connection use IP `172.16.0.101`*