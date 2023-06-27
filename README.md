## Resize storage on OpenWrt Raspberry Pi
> Assuming Openwrt is already installed in your Raspberry-Pi device (if not install from [here](https://github.com/rahulelex/installation-of-openwrt-on-raspberry-pi-4b))
### Step 1. To resize storage on Raspberry Pi, internet access is required to download additional software.
1. Access the OpenWrt WEB interface by opening a browser on your computer by typing http://192.168.1.1
2. Click on drop down menu <Network> and select <Wireless>
3. In Wireless Overview section beside radio0 click on <scan>. It will start wireless scan.
4. Choose your network's SSID having internet access and click on <Join Network>
5. If wireless network is encrypted, Enter your network password in <WPA passphrase> and click submit.
6. Click on save and apply button to connect to this wireless network.
7. To check it internet is working properly. Click on drop down menu <Network> and select <Diagnostics>
8. Under Network Utilities section click on <ping>
9. It will give ping us back as long as we have internet connection.

The Raspberry Pi now has internet access, allowing it to download software.

### Step 3. ssh in the openwrt device
To ssh your Raspberry Pi use below command (make sure Raspberry Pi is connected to your computer using RJ45 cable):
```sh
ssh root@192.168.1.1
```
### Step 4. Install the required packages
```sh
opkg update
opkg install cfdisk resize2fs tune2fs
```
### Step 5. Resize the partition
```sh
cfdisk /dev/mmcblk0
```
Now Resize the /dev/mmcblk0p2 partition (enter desired space). After this write the changes and quit.
```sh
reboot
```
### Step 6. remount root as RO (if fails, reboot and remount as ro again)
```sh
mount -o remount,ro /
```
### Step 7. Remove reserved GDT blocks
```sh
tune2fs -O^resize_inode /dev/mmcblk0p2
fsck.ext4 /dev/mmcblk0p2 
```
(This might probably fail, doesn't seem to affect anything though)
```sh
reboot
```
### Step 8. Resize the f2fs filesystem
```sh 
resize2fs /dev/mmcblk0p2
```
### Step 9. Check new root partition size with:
```sh
df –h
```
This will successfully increase your storage.

## License
**Free Software, Hell Yeah!**

## Authors
- [Rahul Gupta](https://github.com/rahulelex)
