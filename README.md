
My efforts to demystify and control various HDMI matrix hardware...

# Candidate 1
# gofanco HDextIP-TX/RX (1080P Encoder/Decoder) kit based on HDbitT

gofanco HDextIP kit consists of one TX and one RX unit

Comparison of their units here: https://www.gofanco.com/hdbitt-hdmi-extender-comparison-table

![gofanco-front](gofanco-hdextip/gofanco-front.jpg?raw=true "gofanco-front")

![gofanco-rear](gofanco-hdextip/gofanco-rear.jpg?raw=true "gofanco-rear")

![gofanco-bottom](gofanco-hdextip/gofanco-bottom.jpg?raw=true "gofanco-bottom")

Note both the TX and RX unit require 5VDC "wall wort" style power supply

No POE or POC, unless you use a POE splitter such as https://www.amazon.com/dp/B003CFATQK infront of the unit,

or solder a POE power supply PCB inside (there is already the space and through-hole connections for it)

![gofanco-rx-inside](gofanco-hdextip/gofanco-rx-inside.jpg?raw=true "gofanco-rx-inside")
![gofanco-tx-inside](gofanco-hdextip/gofanco-tx-inside.png?raw=true "gofanco-tx-inside")

In my tests I had 2 TX and two RX units on the same LAN...

    192.168.100.101 - TX
    192.168.100.102 - TX
    192.168.100.105 - RX
    192.168.100.106 - RX

when the RX unit is "watching" a TX unit...

    it seems the TX unit (192.168.x.102) is "broadcasting" to 239.255.42.59 (a multicast address)
    UDP port 5004 to destination UDP 5004
    the RX unit is actually at IP 192.168.x.106

doing an IP scan of the entire LAN usually freezes the TX units which have to be rebooted, the HTTP server also stops responding, perhaps this is why they say use a dedicated switch?

each TX unit broadcast traffic comes from it's own address, for example when selecting the "TX connected" via front display on the RX unit...

    ID 11 the traffic comes from 239.255.42.59
    ID 12 the traffic comes from 239.255.42.60
    ID 13 the traffic comes from 239.255.42.61

The IR remote is functional, but strangly will set the TX ID when pointed at both a TX unit or an RX unit.

Seems you would want the TX unit to always stay at the set ID and never change after setup.

But using IP2IR or similar to send the correct codes to individual RX units makes for easy switching

Software from www.hdbitt.com/download-matrix (link obtained from actually reading the HDextIP kit manual)

I could not get the android version to do anything which is dissapointing since the YouTube video on the HDbitT site shows snapshot and streaming previews of every TX unit on the same LAN.  Windows version tries to connect to RX unit on port 9002

Interesting logo served by the TX unit has the same name as one of the ICs...

    http://192.168.100.101/snapshot.jpg

![gofanco-snapshot](gofanco-hdextip/gofanco-snapshot.jpg?raw=true "gofanco-snapshot")

Otherwise the webserver only tells you some version info and the chance to update with a non-existant firmware file...
![gofanco-rx-server](gofanco-hdextip/gofanco-rx-server.png?raw=true "gofanco-rx-server")
![gofanco-tx-server](gofanco-hdextip/gofanco-tx-server.png?raw=true "gofanco-tx-server")

Onscreen when there is no HDMI signal...
![gofanco-onscreen](gofanco-hdextip/gofanco-onscreen.jpg?raw=true "gofanco-onscreen")

Similar info, which I found as result of googling "hdbitt api"...

https://github.com/sure-fire/HDBitT_hdmi_extender

Great info on a very similar unit, which I found as result of googling "IPTV_CMD" (found via Wireshark capture)

https://blog.danman.eu/new-version-of-lenkeng-hdmi-over-ip-extender-lkv373a/

https://github.com/John-K/lkctl

https://cdn2.hubspot.net/hubfs/5334545/VuMATRIX-IP-PRO-control-protocol-V1.0%20(4).pdf


# Candidate 2
# AV Access HDIP100E/D (1080P Encoder/Decoder) kit based on VDirector

Official API document leaves allot to be desired: https://cdn.shopify.com/s/files/1/0260/4934/7646/files/API_Command_Set_HDIP100E_HDIP100D_V1.0.0.pdf

Similar Mfg/OEM of the same device: https://www.alfatronelectronics.com/product/alfatron-alf-ip2he/ + https://www.avpixelfly.com/product/187.html

AV Access only advertises the iPad app found here: https://apps.apple.com/us/app/vdirector/id1499036526

which sadly is only "Compatible with iPad" not phones???

But it seems the real developer is also updating an Android version here: https://apkpure.com/vdirector/com.proitav.vdirector

Without a DHCP server units assign themselves 169.254.x.x

In my tests I had on the LAN...

    192.168.100.40 = Android vDirector App
    192.168.100.60 = Encoder (TX-1)
    192.168.100.19 = Decoder (RX-1)

Video traffic from encoder to decoder all seems to be UDP Src Port 57xxx-58xxx to Dst Port 13000

Control traffic from Android app to decoder seems to be TCP Dst Port 24 (interesting, one away from telet 23)

For example, when you drag the video from TX-1 to RX-1 the following is sent from the App to the Decoder...

    4"E`·n+Ñ%EHu3@@ñÀ¨(À¨³@h8àï½2½!p°%
    ÎMºecho -n "==BEGINSEQ=6==";gbconfig --source-select=341B22810578;gbparam s layout_1;gbparam s layout_2;gbparam s layout_3;gbparam s layout_4;gbparam s layout_5;gbparam s layout_6;gbparam s layout_7;gbparam s layout_8;gbparam s layout_9;gbparam s layout_10;echo -n "==ENDSEQ=6=="

    4"E`·n+Ñ%E5u4@@À¨(À¨³@h8â½2¾>tS
    ÎM.)

    4"E`·n+Ñ%E4u5@@À¨(À¨³@h8â½2¾Nt]Q
    ÎMY,

    4"E`·n+Ñ%E4u6@@À¨(À¨³@h8â½2¾Zt[\
    ÎO\

    4"E`·n+Ñ%E4u7@@À¨(À¨³@h8â½2¾^t[T
    ÎO\

In turn, the Encoder starts sending video to port 13000 on the Decoder
Occasionally you see some UDP traffic to port 14000, not sure what this is.

To stop the stream (drag the video preview back to the Tx List) you get traffic from the App to the Decoder like...

    4"E`·n+Ñ%E@u8@@ôÀ¨(À¨³@h8â½2¾^t>Y
    ÎT\echo -n "==BEGINSEQ=7==";gbconfig --source-select=NULL;gbparam s layout_1;gbparam s layout_2;gbparam s layout_3;gbparam s layout_4;gbparam s layout_5;gbparam s layout_6;gbparam s layout_7;gbparam s layout_8;gbparam s layout_9;gbparam s layout_10;echo -n "==ENDSEQ=7=="

    4"E`·n+Ñ%E5u9@@þÀ¨(À¨³@h8ã½2¿sxI!
ÎTè

    4"E`·n+Ñ%E4u:@@þÀ¨(À¨³@h8ã½2¿xRé
ÎT¾é

    `·n+Ñ%4"EE8£å@@éNÀ¨À¨(³@½2¿h8ãþþ
&ÎV÷/ # 

You can get a motion JPG image preview from the Encoder with a URL like...
 
    http://192.168.100.60/stream?resolution=1080P&fps=20&bitrate=128
    http://192.168.100.60/stream

A google search for the control string "gbconfig --source-select" seems to indicate other units also use this same code...

https://www.conferenceroomav.com/pdf/sw-0501-hdbt-installation.pdf

https://www.infobitav.com/wp-content/uploads/2021/02/Telnet-commands-for-iShare-Plus.pdf

Of interesting note (from the API document) is the ability to change the NO SOURCE default image...

    http://192.168.100.19/upload_bg (HTTP POST)


# Candidate 3
# NoHassleAV 8x8-HDbaseT-matrix

Comming soon


# Candidate 4
# NoHassleAV8x8-HDbaseT-150m-matrix

Comming soon
