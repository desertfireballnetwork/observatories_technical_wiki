How actually do VPNs work? Each VPN client (the observatory, your laptop
or workstation) gets a sort of 'virtual network card' that is connected
through the Internet to a VPN server which sits on a static public IP
address. It's a star shape structure\*. Each client periodically asks
the server whether someone else within the VPN wants to talk to it.
Example: when I ping to the DFNEXT observatory you deployed, ping
10.1.23.SERIALNUMBER (in case of DFNEXT type) or 10.1.17.SERIALNUMBER
(in case of DFNSMALL type) 10.1.21.SERIALNUMBER (in case of DFNKIT
type), the ping packet goes to our VPN server in Perth, where it waits
for the observatory to pick it up. When it does, the ping packet is
finally delivered to the target host. Camera generates a reply tp ping
and uploads it to the VPN server with public IP, now the original ping
initiator picks up the reply and knows the observatory is alive and
on-line. All this is transparent and the communication is encrypted. It
does not matter where the VPN clients are and whether they are behind
NAT or firewall blocking communication coming from outside, as long a
they have access to the Internet (no firewall blocking outgoing
communication) to be able to check for messages on the VPN server.

In our case, the DFN VPN server is in Perth, Western Australia, so all
the communication between VPN clients somewhere elsewhere on the planet
has to travel quite a bit around which results in noticeable latencies
(it could be eg ~500-800ms if all is good) and the transfer rate will
not be too fast as well.

We suppose that this limitations might not be practical for your
regional fireball network. It may be ok for occasional checking of the
cameras state, but not for eg transferring large volumes of data data
like RAW images. Therefore we encourage you to set up a local VPN,
especially if you planned to deploy many cameras in frame of building
your local network. It can co-exist with the default DFN VPN as long as
you use different IP address range (DFN VPN uses
10.1.16.0/255.255.240.0). We have a good experience and can recommend
[Open VPN](wikipedia:OpenVPN.md "wikilink") <https://openvpn.net/>.

The other thing is that our VPN server is hosted by a commercial
provider and pay per transferred data. That means transferring bigger
volumes you are going to ramp up our monthly bill.

------------------------------------------------------------------------

\*) The star topology of VPN means that all the communication between
the numerous VPN clients travels through the single VPN server which is
a sort of post office. If you draw the paths of data packets traveling,
you'd get a star like shape.