Nidage Borescope Reverse Engineering
====================================

Dumped the spi flash with a soic16 chip clip and a buspirate. mcrypt'ed fs and some other rando junk. DHCP, UDP/8031, and UDP/49159 are open when the device boots. NC'ed into 8031, but no juice, probably some type of handshaking is required before it start spitting the mjepg stream. 

Snagged a "clean" copy of "HD WIFI" from apkpure, unzipped, found a buncha junk but nothing that looks state actor. Nostra13/universal-image-lib and some janky ass vanilla Android SDK UI stuff. libmlcamera looks like it has juice. wifi handling is all vanilla android. Dumped the apk with apktools too, nothing else popped up. Also some smali stuff for CMDSocket which for sure looks like the juice for 8031. Osint on the dev shows a treasure trail of similar products, all with different corp names but all the same physical addresses.

Went down a rabbithole on Supertek M8189 and Trolink TL8189 modules. Both are just cheap shenzen modules using RTL8189FTV for all lifting. There is also a mystery QFP48 with some non-standard marking, zero whitepapers or hits from oconus on its nums. Its got pogo pads for possily uart, im gonna solder some clippie loops on and see if I can pull anything.

Updated: udp/8031 takes cmdset options from socketcmd java found in the "HD WIFI" apk. Need to build a python cmd handler->hex gen->udp datagram gen to try fuzzing 8031 and 49159 with stuff I found in the apk. wip... bleh, almost there.

![chipclip](/images/nidage-dump.jpg?raw=true)
