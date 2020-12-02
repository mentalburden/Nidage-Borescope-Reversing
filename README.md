Nidage Borescope Reverse Engineering
====================================

Day1:
Dumped the spi flash with a soic16 chip clip and a buspirate. mcrypt'ed fs and some other rando junk. Network enum on the device shows DHCP, UDP/8031, and UDP/50000 are open when the device boots. NC'ed into 8031, but no juice, probably some type of handshaking is required before it starts spitting the mjepg stream. 

Day2:
Snagged a "clean" copy of "HD WIFI" from apkpure, unzipped, found a buncha junk but nothing that looks state actor. Nostra13/universal-image-lib and some janky ass vanilla Android SDK UI stuff. Wifi handling is all vanilla android. libmlcamera_2.2.so looks like it has juice. Threw it into ghidra for the strings and found some manufacturer/dev details. Osint on the dev shows a treasure trail of similar products, all with different corp names but all the same physical addresses. The handful of APKs for similar products are almost identical to this one, probably industry/trend hopping. Dumped the apk with apktools too, nothing else popped up. Also some smali stuff for CMDSocket which for sure looks like the juice for 8031 and 50000. 

Day2.5:
Went down a rabbithole on Supertek M8189 and Trolink TL8189 modules. Both are just cheap shenzen modules using RTL8189FTV for all lifting. There is also a mystery QFP48 with some non-standard marking, zero whitepapers or hits from oconus on its nums. The back of the board has pogo pads, im gonna solder some clippie loops on and see if I can pull uart or something.

Day 3: udp/50000 takes CMDSET options from socketcmd java found in the "HD WIFI" apk. Need to build a python cmd handler->hex gen->udp datagram gen to try fuzzing 8031 and 50000 with stuff I found in the apk. No joy on pogo clippies, threw a DS0 and logic anny at them and they all appear to be voltage test points and freq check points for the wifi stuff. 

Day 4: FINALLY got UDP/8031 to spit an mjpeg'ish looking stream! Yaaaaaay! Had to hit it with a start packet I found by running a monitor on the wifi this thing provides (should have wiresharked this from the begining... fail). During wifi monitoring found what appears to be vestigial traffic going to 7060, but the port is shut on the device and there was no response ever seen. For tru-pos reassurance I ran pcap on the burner phone with the apk installed, verified all the ports are correct and the init/start udp packet from the app is the same. Going to build a scapy script to handle this to dump video into vlc (might be able to one line it). Then maybe look into webasm to handle this for a browser version, like the developer should have built in the first f'ing place.

![front board](/images/nidage-front.jpg?raw=true)
![back board](/images/nidage-back.jpg?raw=true)
![chipclip](/images/nidage-dump.jpg?raw=true)

Wireshark time:

![startpkt](/images/nidage-packet-start.png?raw=true)
![setcmd](/images/nidage-packet-setcmd.png?raw=true)
![retcmd](/images/nidage-packet-retcmd.png?raw=true)
