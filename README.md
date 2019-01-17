# rock64-arch-latest
prepare sdcard for boot rock64 sbc

## sources

```txt
https://archlinuxarm.org/platforms/armv8/rockchip/rock64#installation
```


## HANDLE WITH CARE


```bash
# install bsdtar gnu will not work
sudo apt-get install bsdtar

# delete sdcard
sudo dd if=/dev/zero of=/dev/sdb bs=1M count=32 status=progress

# write partion on sdcard
sudo fdisk /dev/sdb <<EOF
o
n
p
1
32768

w
EOF


sudo mkfs.ext4 /dev/sdb1
sudo mount /dev/sdb1 /mnt

wget http://os.archlinuxarm.org/os/ArchLinuxARM-aarch64-latest.tar.gz
sudo bsdtar -xpf ArchLinuxARM-aarch64-latest.tar.gz -C /mnt
wget http://os.archlinuxarm.org/os/rockchip/boot/rock64/idbloader.img
wget http://os.archlinuxarm.org/os/rockchip/boot/rock64/uboot.img
wget http://os.archlinuxarm.org/os/rockchip/boot/rock64/trust.img
sudo dd if=idbloader.img of=/dev/sdb seek=64 conv=notrunc
sudo dd if=uboot.img of=/dev/sdb seek=16384 conv=notrunc
sudo dd if=trust.img of=/dev/sdb seek=24576 conv=notrunc

```
```

## boot from sdcard or disk if epi boot active 


