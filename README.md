# arch-linux  
https://wiki.archlinux.org/  
Установка арча на ноут acer 8G 2x500G HDD  

localectl list-keymaps   
loadkeys ru  
setfont cyr-sun16  
cat /sys/firmware/efi/fw_platform_size  
ip link  
ping archlinux.org  
#####
iwctl  
device list  
station wlan0 scan  
station wlan0 get-networks  
station wlan0 connect SSID  
####
timedatectl  
pacman -S mdadm
fdisk -l  
wipefs --all --force /dev/sda /dev/sdb  
fdisk /dev/sda  
1G esp (t) + all
fdisk /dev/sdb  
1G swap (t) + all  
mdadm --create --verbose /dev/md127 --chunk=128 --level=0 --raid-devices=2 /dev/sda2 /dev/sdb2
mkfs.ext4 -b 4096 -E stride=32,stripe-width=64 /dev/md127  
mkswap /dev/sdb1  
swapon /dev/sdb1  
mkfs.fat -F 32 /dev/sda1  
mount /dev/md127 /mnt  
mount --mkdir /dev/sda1 /mnt/boot  
pacstrap -K /mnt base linux-lts linux-lts-headers linux-firmware base-devel nano grub efibootmgr dhcpcd dhclient networkmanager intel-ucode iucode-tool xfce4 xfce4-goodies sddm mdadm  
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt  
####
ln -sf /usr/share/zoneinfo/Europe/Moscow /etc/localtime  
hwclock --systohc  
nano /etc/locale.gen  
locale-gen  
echo "LANG=ru_RU.UTF-8" | tee -a /etc/locale.conf
nano /etc/vconsole.conf  
KEYMAP=ru  
FONT=cyr-sun16  
echo "acer" | tee -a /etc/hostname  
mdadm --detail --scan | sudo tee -a /etc/mdadm.conf
nano /etc/mkinitcpio.conf
mkinitcpio -P  
passwd  
grub-install /dev/sda  
grub-mkconfig -o /boot/grub/grub.cfg  
exit  
umount -R /mnt  
