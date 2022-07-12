  Hacking the Infomir MAG254 Set-Top Box
==========================================

I ran across this set top box at a garage sale and thought I'd give it a go.

Parts

- CPU ST Micro STxH205
- SPI m25px80 8 Mb NOR Serial Flash
- ONFI flash is S34ML02G1, 256 MiB, page size 2048+64, sector size 128 KiB
- 256 MiB
- NAND: AMD S34ML02G1 2 Gb
- RAM 256 MB SEC?
- drv632 ti audio line driver
- tps65581 ti PMIC
- k4b2g1646f bck0  Samsung DDR3 SDRAM
- 25px80vp st
- pn521 ti hdmi

Software

- U-Boot 2010.03-svn186 (May 22 2017 - 22:13:21) - stm24_0120
- Linux version 2.6.32.59_stm24_0211-MAG255

1. I was able to log into mine via:

```
ssh root@192.168.2.233 (adjust the address)
```
Password: 930920

2. It has the ability to run either NAND or NAND2 banks. Once I updated it I could only login when running NAND2

3. Links you might find useful

https://soft.infomir.com/mag254/release/
https://geniptv.net/threads/mag-410.880/page-3
https://wiki.infomir.eu/eng/set-top-box/stb-linux-webkit/mag250-254-270
https://wiki.infomir.eu/eng/set-top-box/for-developers/stb-linux-webkit/customization/ssh-change-password-port-disable-enable

4. You can hook up a serial port (see pictures) but you need to ssh in first and change /etc/inittab from:

```
::sysinit:/bin/sh /web.sh
ttyS1::respawn:/sbin/getty -L ttyS1 115200 vt100
```

to:

```
::respawn:/bin/sh /web.sh
ttyAS0::respawn:/sbin/getty -L ttyAS0 115200 vt100
```

5. copying one of the NAND partitions (do this from the MAG254 itself):

dd if=/dev/mtdblock6 | ssh rick@192.168.2.9 "dd of=/home/rick/boards/mag254/mtd6.bin"

6. Trying IPTV

  Put this on a thumb drive:

```
wget https://iptv-org.github.io/iptv/index.m3u
```

  and insert it into the MAG254 and do:
	
  Then from the remote do "Home Media"->[USB thumb drive]->index.m3u->OK->F3

  Then back out (via the button with the house icon) and go to "IPTV Channels"->OK

7. Comments/suggestions

  Please contact me at rick AT efn DOT org

BottomCase
![alt text](https://github.com/rickbronson/Hacking-the-Infomir-MAG254/tree/master/docs/hardware/BottomCase.png "BottomCase")
BottomSidePC
![alt text](https://github.com/rickbronson/Hacking-the-Infomir-MAG254/tree/master/docs/hardware/BottomSidePC.png "BottomSidePC")
TopSidePC
![alt text](https://github.com/rickbronson/Hacking-the-Infomir-MAG254/tree/master/docs/hardware/TopSidePC.png "TopSidePC")
