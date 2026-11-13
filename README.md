.............................................................
Practica 1
.............................................................
ip a
sudo exportfs -rav
sudo exportfs -v
sudo firewall-cmd --add-service=nfs --permanent
sudo firewall-cmd --reload
showmount -e 192.168.106.133
sudo mount -t nfs 192.168.106.133:/OS3 /mnt
df -h | grep OS3
sudo mount -a
sudo systemctl daemon-reload
df -hT /mnt/OS3
findmnt /mnt/OS3
touch /mnt/OS3/prueba_cliente.txt
ls -l /mnt/OS3/prueba_cliente.txt
sudo touch /mnt/OS3/archivo_root.txt
ls -l /mnt/OS3/archivo_root.txt
.............................................................
Practica 2
.............................................................
whoami
hostname
cat /etc/redhat-release
ip -4 a
sudo dnf install -y samba samba-client samba-common-tools policycoreutils-python-utils
systemctl status smb --no-pager
sudo groupadd smbgrupo
sudo usermod -aG smbgrupo lsantos
newgrp smbgrupo
id
sudo mkdir -p /srv/samba/compartida
sudo chown root:smbgrupo /srv/samba/compartida
sudo chmod 2770 /srv/samba/compartida
ls -ld /srv/samba/compartida
getenforce
ls -Zd /srv/samba/compartida
sudo semanage fcontext -a -t samba_share_t "/srv/samba/compartida(/.*)?"
sudo restorecon -Rv /srv/samba/compartida
ls -Zd /srv/samba/compartida
echo "Hola desde RHEL Leslie 20261154" | sudo tee /srv/samba/compartida/bienvenida.txt
sudo chmod 660 /srv/samba/compartida/bienvenida.txt
ls -l /srv/samba/compartida/
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
sudo nano /etc/samba/smb.conf
testparm
sudo smbpasswd -a lsantos
sudo pdbedit -L
sudo systemctl enable --now smb
systemctl status smb --no-pager
sudo firewall-cmd --permanent --add-service=samba
sudo firewall-cmd --reload
sudo firewall-cmd --list-services
smbclient -L localhost -U lsantos
ls -l /srv/samba/compartida/
sudo smbstatus
sudo useradd invitado
sudo smbpasswd -a invitado
sudo smbpasswd -x invitado
sudo userdel -r invitado

////////////////////////////////////////////////////////////////////
Comandos en Windows 

:: 1. Probar conexión con el servidor
ping ip de su maquina

:: 12. Montar y verificar unidad de red
net use Z: \\ip de su maquina\compartida
net use

:: 15. Prueba de acceso denegado (Opcional)
net use * /delete /y
net use Z: \\ip de su maquina\compartida /user:invitado *
.............................................................
Practica 3 
.............................................................
sudo useradd lanegracubana
echo 'lanegracubana:20261154' | sudo chpasswd
id lanegracubana
sudo hostnamectl set-hostname dc-20261154.so3.inet
echo "192.168.106.10 dc-20261154.so3.inet dc-20261154" | sudo tee -a /etc/hosts
hostname -f
sudo nmcli con mod ens33 ipv4.method manual ipv4.addresses 192.168.106.10/24 ipv4.gateway 192.168.106.2 ipv4.dns 192.168.106.2 ipv4.ignore-auto-dns yes
sudo nmcli con up ens33
ip -4 a show ens33
cat /etc/resolv.conf
ping -c 3 google.com
sudo setenforce 0
sudo sed -i 's/^SELINUX=enforcing/SELINUX=permissive/' /etc/selinux/config
getenforce
sudo firewall-cmd --permanent --add-port=53/tcp --add-port=53/udp --add-port=88/tcp --add-port=88/udp --add-port=135/tcp --add-port=139/tcp --add-port=389/tcp --add-port=389/udp --add-port=445/tcp --add-port=464/tcp --add-port=464/udp --add-port=636/tcp --add-port=3268/tcp --add-port=3269/tcp --add-port=49152-65535/tcp
sudo firewall-cmd --reload
sudo firewall-cmd --list-ports
timedatectl
sudo systemctl disable --now smb nmb winbind 2>/dev/null
sudo curl -fsSL -o /etc/pki/rpm-gpg/RPM-GPG-KEY-TISSAMBA-10 https://samba.tranquil.it/RPM-GPG-KEY-TISSAMBA-10
sha256sum /etc/pki/rpm-gpg/RPM-GPG-KEY-TISSAMBA-10
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-TISSAMBA-10
sudo tee /etc/yum.repos.d/tissamba.repo > /dev/null << 'EOF'
[tis-samba]
name=tis-samba
baseurl=https://samba.tranquil.it/redhat10/samba-4.24/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-TISSAMBA-10
priority=1
EOF
sudo subscription-manager repos --enable codeready-builder-for-rhel-10-$(arch)-rpms
sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-10.noarch.rpm
dnf repolist
sudo dnf install -y samba samba-dc samba-winbind samba-winbind-clients samba-client krb5-workstation
rpm -q samba samba-dc python3-setproctitle
sudo mv /etc/samba/smb.conf /etc/samba/smb.conf.bak
sudo samba-tool domain provision --realm=SO3.INET --domain=SO3 --server-role=dc --dns-backend=SAMBA_INTERNAL --adminpass='Admin2026*'
sudo cp /var/lib/samba/private/krb5.conf /etc/krb5.conf
grep "dns forwarder" /etc/samba/smb.conf
sudo nmcli con mod ens33 ipv4.dns 127.0.0.1
sudo nmcli con up ens33
cat /etc/resolv.conf
sudo systemctl enable --now samba
systemctl is-active samba
host -t SRV _ldap._tcp.so3.inet
host -t A dc-20261154.so3.inet
kinit administrator@SO3.INET
klist
smbclient -L dc-20261154.so3.inet --use-kerberos=required
sudo samba-tool domain passwordsettings set --complexity=off
sudo samba-tool user create lanegracubana 20261154
sudo samba-tool user list
wbinfo -u
sudo samba-tool computer list

///cmd////

ipconfig
systeminfo | findstr /C:"Nombre del sistema operativo"
changepk.exe /ProductKey VK7JG-NPHTM-C97JM-9MPGT-3V66T
ncpa.cpl
ipconfig /flushdns
ipconfig /all
ping 192.168.106.10
nslookup so3.inet
nslookup -type=SRV _ldap._tcp.so3.inet
sysdm.cpl
whoami
echo %LOGONSERVER%
