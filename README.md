Nidage Borescope Reverse Engineering
====================================

Dumped the spi flash with a soic16 chip clip and a buspirate. mcrypt'ed fs and some other rando junk. Found some juice and mention of http with hexdump and strings, gonna ghidra the "HD WIFI" apk another day. DHCP and UDP/8031 are open when the device boots. NC'ed into 8031, but no juice, probably some type of handshaking is required before it start spitting the mjepg stream. 

![chipclip](/images/nidage-dump.jpg?raw=true)
