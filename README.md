Nidage Borescope Reverse Engineering
====================================

Day1:
Dumped the spi flash with a soic16 chip clip and a buspirate. mcrypt'ed fs and some other rando junk. Network enum on the device shows DHCP, UDP/8031, and UDP/50000 are open when the device boots. NC'ed into 8031, but no juice, probably some type of handshaking is required before it start spitting the mjepg stream. 

Day2:
Snagged a "clean" copy of "HD WIFI" from apkpure, unzipped, found a buncha junk but nothing that looks state actor. Nostra13/universal-image-lib and some janky ass vanilla Android SDK UI stuff. libmlcamera_2.2.so looks like it has juice. Threw it into ghidra for the strings and found some manufacturer/dev details. Wifi handling is all vanilla android. Dumped the apk with apktools too, nothing else popped up. Also some smali stuff for CMDSocket which for sure looks like the juice for 8031. Osint on the dev shows a treasure trail of similar products, all with different corp names but all the same physical addresses.

Day2.5:
Went down a rabbithole on Supertek M8189 and Trolink TL8189 modules. Both are just cheap shenzen modules using RTL8189FTV for all lifting. There is also a mystery QFP48 with some non-standard marking, zero whitepapers or hits from oconus on its nums. Its got pogo pads for possily uart, im gonna solder some clippie loops on and see if I can pull anything.

Day 3: udp/50000 takes CMDSET options from socketcmd java found in the "HD WIFI" apk. Need to build a python cmd handler->hex gen->udp datagram gen to try fuzzing 8031 and 50000 with stuff I found in the apk.

Day 4: FINALLY got UDP/8031 to spit an mjpeg'ish looking stream. Had to hit it with a start packet I found by running a monitor on the wifi net (should have wiresharked this from the begining... fail). Going to build a scapy script to handle this to dump video into vlc (might be able to one line it). Then maybe look into webasm to handle this for a browser version. Looking like the APK has some vestigial traffic going to 7060 (found in other similar products).

![chipclip](/images/nidage-dump.jpg?raw=true)
