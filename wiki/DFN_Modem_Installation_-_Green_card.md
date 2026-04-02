***These instructions are for DFN camera systems equipped with green PCB
modem adaptor board (see images below). For DFNEXT observatories with
blue PCB modem adaptor board please refer to [DFNEXT Modem
Installation](DFNEXT_Modem_Installation "wikilink").***

The DFNEXT / DFNSMALL observatories usually ship without modems as many
of them are deployed with WiFi or Ethernet connectivity and different
areas of the world require different modems.

If you wish to network your DFNEXT observatory via mobile broadband
instead of WiFi or Ethernet you need to source a modem and request a
modem installation care package from DFN Camera Help. You will also need
to organize a 2FF ("standard") sized SIM with an active data plan.

The recommended modems are the Sierra Wireless AirPrime MC Series.
MC7455 (for the Americas, Europe, the Middle East and Africa) and the
MC7430 (for Asia Pacific including Australia). For Australia, MC8705 is
suitable as well. It is also a good idea to verify that the selected
modem will support the frequencies/bands used by your operator in the
area.

# Installation Steps

Before modem installation make sure the camera is powered off. If the
PCB modem adaptor board is already in the camera (assuming without the
modem module), remove the min USB power connector and pull the board
out.

Install the modem module into the adaptor board (converter card) and fix
it using M2.5 bolt and nuts.

<figure>
<img
src="Green_converter_card_and_Sierra_MC8705_modem_module_next_to_it.jpg"
title="File:Green_converter_card_and_Sierra_MC8705_modem_module_next_to_it.jpg" />
<figcaption><a
href="File:Green_converter_card_and_Sierra_MC8705_modem_module_next_to_it.jpg">File:Green_converter_card_and_Sierra_MC8705_modem_module_next_to_it.jpg</a></figcaption>
</figure>

A pair of bolts goes from bottom of the adopter board, fasten the first
(bottom) nut or insert spacer on each of them, insert the modem module
and fasten the upper nuts.

<figure>
<img
src="Green_converter_card_with_Sierra_MC8705_modem_module_in_slot.jpg"
title="Green_converter_card_with_Sierra_MC8705_modem_module_in_slot.jpg"
width="400" />
<figcaption>Green_converter_card_with_Sierra_MC8705_modem_module_in_slot.jpg</figcaption>
</figure>

Next step is to insert the SIM card. First open the slot door and slide
the SIM card into it.

<figure>
<img src="Green_converter_card_SIM_slot_open.jpg"
title="Green_converter_card_SIM_slot_open.jpg" width="400" />
<figcaption>Green_converter_card_SIM_slot_open.jpg</figcaption>
</figure>

Then shut the slot door with SIM card and lock it - see arrows on the
slot door.

<figure>
<img src="Green_converter_card_with_SIM_card_in_slot.jpg"
title="Green_converter_card_with_SIM_card_in_slot.jpg" width="400" />
<figcaption>Green_converter_card_with_SIM_card_in_slot.jpg</figcaption>
</figure>

Attach the GSM antenna RF patch cable (pigtail) to the main antenna port
on the modem (marked by red arrow) using your fingers or some
needle-nose pliers. Ensure the U.FL connector is seated correctly.

Slide the whole modem assembly back to the slot in the DFNEXT
observatory box.

Attach the mini USB cable connecting the modem with embedded PC.

# Network configuration

Make sure the networking is configured correctly for a specific modem
type and operator. Please contact your service provider for details like
AP name (example: "telstra.m2m" for Australian Telstra M2M plans),
Password, Username and Phone (number) to dial.

The [Mobile network
configuration](Mobile_network_configuration "wikilink") in the
observatory PC operating system has moved to a separate wiki page.