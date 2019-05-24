So assuming you installed 16.04 LTS, kernel version 4.4 or similar, check if on entering this command :
```
sudo lshw -c network
```
if you see something like this : 
```
-network UNCLAIMED     
       description: Network controller
       product: Wireless 8265 / 8275
```
confirm that your hardware is being detected by trying this : 
```
lspci -nnk | grep 0280 -A3
```
you should see something like this : (if not then i don't really know what to do about it, sorry :P )
```
00:14.3 Network controller [0280]: Intel Corporation Wireless-AC 9560 [Jefferson Peak] [8086:a370] (rev 10)
	Subsystem: Intel Corporation Device [8086:0034]
	Kernel driver in use: iwlwifi
```
iif you're with me so far, and no other fixes have worked for you, try the following commands (one by one, obviously)
BUT. Before you execute this, know that the reason why i am backporting from 5.0-rc6 is because it contains the wifi driver stuff
series 9000 intel wifi cards. If you have series 8000 intel wifi cards, you can change 5.0-rc6 to 4.4.2 and keep the rest same.
```
wget https://www.kernel.org/pub/linux/kernel/projects/backports/stable/v5.0-rc6/backports-5.0-rc6-1.tar.gz
tar xvf backports-5.0-rc6-1.tar.gz
cd backports-5.0-rc6-1/
make defconfig-iwlwifi
make
sudo make install
```
you will now have to reboot your computer. When you reboot it, press F2 (for acer laptops) or F12 to enter the BIOS menu
in there, you will have to disable the secure boot setting in the boot section, however, it may be greyed out(you may not be able
to change it). To be able to change it, you will have to set a superviser password in the security section (write it down somewhere
so you don't forget it) and then you'll be able to disable secure boot. Once you do that, exit after saving and power it up again.
Once you have logged in, on the terminal : 
```
sudo modprobe iwlwifi
```
will do the job.
