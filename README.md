

==========================================
 PRÁCTICA 1 - GRUB2
==========================================


sudo nano /etc/default/grub
GRUB_TIMEOUT=20
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
sudo reboot
rd.break
mount -o remount,rw /sysroot
chroot /sysroot
passwd root
touch /.autorelabel
exit
exit
su -
exit

==========================================
PRÁCTICA 2 - SCRIPTS BASH
==========================================

SCRIPT 1 

nano cp_home.sh
#!/bin/bash

FECHA=$(date +"%d-%m-%Y:%H:%M")
USUARIO=$(whoami)
tar -czvf "/tmp/cp_home_$FECHA.tar.gz" -C /home "$USUARIO"
echo "Backup creado: /tmp/cp_home_$FECHA.tar.gz"
chmod +x cp_home.sh
./cp_home.sh
ls -lh /tmp/cp_home_*.tar.gz

SCRIPT 2 

sudo dnf install -y net-tools
mkdir -p ~/Desktop
nano ifconf.sh
#!/bin/bash
read -p "Nombre del archivo: " nombre
ifconfig > "$HOME/Desktop/$nombre.txt"
echo "Archivo creado: $nombre.txt"
chmod +x ifconf.sh
./ifconf.sh
cat ~/Desktop/red_rhel.txt


==========================================
PRÁCTICA 3 - BRIDGE Y SSH
==========================================


ip a
sudo dnf install -y openssh-server
sudo systemctl enable --now sshd
sudo systemctl status sshd
ping direccion ip
ssh lsantos@dirección ip
exit

ssh-keygen
mkdir -p ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
ssh dirección ip
exit
