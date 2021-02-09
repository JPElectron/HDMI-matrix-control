
My efforts to demystify and control various HDMI matrix hardware


#gofanco HDextIP based on HDbitT

gofanco HDextIP kit consists of one TX and one RX unit
Comparison of their units here: https://www.gofanco.com/hdbitt-hdmi-extender-comparison-table

![gofanco-front](gofanco-hdextip/gofanco-front.jpg?raw=true "Front")

Note both the TX and RX unit require 5VDC "wall wort" style power supply
No POE or POC, unless you use a POE splitter such as https://www.amazon.com/dp/B003CFATQK infront of the unit, or solder a POE power supply PCB inside (there is already the space and through-hole connections for it)

when the RX unit is "watching" a TX unit...
it seems the TX unit (192.168.x.102) is "broadcasting" to 239.255.42.59 (a multicast address)
UDP port 5004 to destination UDP 5004
even though the RX unit is actually at IP 192.168.100.106

doing an IP scan of the entire LAN usually freezes the TX units which have to be rebooted
their HTTP server also stops responding

each TX unit broadcast traffic comes from it's own address,
for example when selecting the "TX connected" via front display on the RX unit...
ID 11 the traffic comes from 239.255.42.59
ID 12 the traffic comes from 239.255.42.60
ID 13 the traffic comes from 239.255.42.61

The IR remote is functional, but strangly will set the TX ID when pointed at both a TX unit or an RX unit.
Seems you would want the TX unit to always stay at the set ID and never change after setup.
But using IP2IR or similar to send the correct codes to individual RX units makes for easy switching

Software from www.hdbitt.com/download-matrix (link obtained from actually reading the HDextIP kit manual)
I could not get the android version to do anything which is dissapointing since the YouTube video on the HDbitT site shows snapshot and streaming previews of every TX unit on the same LAN.
Windows version tries to connect to RX unit on port 9002

Interesting logo served by the TX unit http://192.168.x.101/snapshot.jpg has the same name as one of the ICs.


Similar info, which I found as result of googling "hdbitt api"...

https://github.com/sure-fire/HDBitT_hdmi_extender

Great info on a very similar unit, which I found as result of googling "IPTV_CMD" (found via Wireshark capture)

https://blog.danman.eu/new-version-of-lenkeng-hdmi-over-ip-extender-lkv373a/

https://github.com/John-K/lkctl

https://cdn2.hubspot.net/hubfs/5334545/VuMATRIX-IP-PRO-control-protocol-V1.0%20(4).pdf
