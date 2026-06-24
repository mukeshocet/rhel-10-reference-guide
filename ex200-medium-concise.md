# RHCSA EX200 Practice Guide (RHEL 10) - Concise Medium Version

All questions and commands are preserved. Extra explanation is minimized for quick publish/read.

## Question 1: Network and Hostname

```bash
nmtui
```
```bash
nmcli connection show
ip a
nmcli device
nmcli connection modify <profile> ipv4.addresses <ip/prefix> ipv4.gateway <gateway> ipv4.dns <dns> ipv4.method manual
nmcli connection up <profile>
hostnamectl set-hostname <hostname>
```

## Question 2: Repository Configuration (BaseOS/AppStream)

```ini
[BaseOS]
name=BaseOS
baseurl=http://repo.lab.example.com/rocky9.5/repo/BaseOS
enabled=1
gpgcheck=0

[AppStream]
name=AppStream
baseurl=http://repo.lab.example.com/rocky9.5/repo/AppStream
enabled=1
gpgcheck=0
```
Validation:
```bash
dnf clean all
dnf repolist all
dnf search httpd
```

## Question 3: HTTP on Port 82

```bash
dnf install -y httpd
systemctl enable --now httpd
semanage port -a -t http_port_t -p tcp 82
firewall-cmd --permanent --add-port=82/tcp
firewall-cmd --reload
curl http://localhost:82
```

## Question 4: Users and Groups

```bash
groupadd admin
useradd -G admin harry
useradd -G admin natasha
useradd -s /sbin/nologin sarah
passwd harry
passwd natasha
passwd sarah
```

## Question 5: Shared Directory with SGID

```bash
mkdir -p /common/admin
chgrp admin /common/admin
chmod 770 /common/admin
chmod g+s /common/admin
```

## Question 6: NFS Auto-mount with Timeout

`/etc/auto.master`
```text
/automount /etc/auto.nfs --timeout=30
```
`/etc/auto.nfs`
```text
public  -ro,sync  nfs.lab.example.com:/public
private -rw,sync  nfs.lab.example.com:/private
```
```bash
systemctl enable --now autofs
```

## Question 7: Cron + Deny User

Crontab:
```cron
30 12 * * * /bin/echo "EX200"
```
Deny:
```bash
echo natasha >> /etc/cron.deny
```

## Question 8: ACL Task

```bash
cp /etc/fstab /var/tmp/
setfacl -m u:harry:rw /var/tmp/fstab
setfacl -m u:natasha:--- /var/tmp/fstab
getfacl /var/tmp/fstab
```

## Question 9: NTP / Chrony

```bash
dnf install -y chrony
systemctl enable --now chronyd
systemctl restart chronyd
chronyc sources
timedatectl
```

## Question 10: Find Files >4MB and Copy

```bash
mkdir -p /find/largefiles
find /etc -type f -size +4M -exec cp {} /find/largefiles/ \;
```

## Question 11: User with UID

```bash
useradd -u 6969 billy
passwd billy
```

## Question 12: Archive and Compress

```bash
# gzip
tar -zcvf /root/ex200.tar.gz /var/tmp

# bz2
tar -jcvf /root/ex200.tar.bz2 /var/tmp

# xz
tar -Jcvf /root/ex200.tar.xz /var/tmp
```

## Question 13: UMASK

```bash
su - natasha
umask 0277
```

## Question 14: Password Expiry for New Users

`/etc/login.defs`
```text
PASS_MAX_DAYS 20
```

## Question 15: Passwordless sudo for admin Group

```text
%admin ALL=(ALL) NOPASSWD: ALL
```
```bash
sudo -l
```

## Question 16: Script Practice

```bash
#!/bin/bash
find /usr/share -type f -size -1M -exec cp -a {} /root/find \;
```

## Question 17: Root Password Reset (Recovery)

```bash
passwd root
touch /.autorelabel
exec /sbin/init
```

## Question 18: 512MB Swap

```bash
lsblk
fdisk /dev/sdb
mkswap /dev/sdb1
swapon -a
```

## Question 19: LVM with Extents

```bash
pvcreate /dev/sdb2
vgcreate -s 8M datastore /dev/sdb2
lvcreate -l 50 -n database datastore
mkfs.ext4 /dev/datastore/database
```
```bash
lvcreate -L 2G -n database datastore
```

## Question 21: Extend LV by 100 Extents

```bash
lvextend -l +100 -r /dev/datastore/database
```

## Question 22: tuned Profile

```bash
dnf install -y tuned
systemctl enable --now tuned
tuned-adm recommend
tuned-adm active
```

## Question 24: systemd Timer

Exam scope note: RHEL 10 only.
```ini
[Timer]
OnCalendar=hourly
Persistent=true
```
```bash
systemctl daemon-reload
systemctl enable --now testtimer.timer
```

## Question 25: Flatpak Remote

Exam scope note: RHEL 10 only.
```bash
dnf install -y flatpak

# System-wide remote
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# User-only remote
su - <username>
flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

## Question 26: Script Argument Order

```bash
#!/bin/bash
if [ $# -eq 2 ]; then
  echo "$2 $1"
else
  echo "Usage: $0 argument1 argument2"
  exit 1
fi
```

## Question 27: Default File for New Users

```bash
touch /etc/skel/Congrats
chmod 644 /etc/skel/Congrats
```

## Question 28: Password Policy

`/etc/login.defs`
```text
PASS_MAX_DAYS 90
```
`/etc/security/pwquality.conf`
```text
minlen = 8
```

## Question 29: Passwordless Root SSH (Lab Scenario)

```bash
ssh-keygen
ssh-copy-id root@serverb
ssh root@serverb
```

## RHEL 9 Only) Containers with Podman

```bash
dnf search podman
dnf install -y podman container-tools
podman login registry.lab.example.com --tls-verify=false
```
```bash
su - harry
podman run -d --name watch registry.access.redhat.com/ubi8/ubi
podman ps
```
```bash
podman pull <IMAGE_URL>
podman tag <IMAGE_URL> watch
podman create --name watch_c watch
podman generate systemd --name watch_c --files --new
mkdir -p ~/.config/systemd/user
mv container-watch_c.service ~/.config/systemd/user/
systemctl --user daemon-reload
systemctl --user enable --now container-watch_c.service
sudo loginctl enable-linger harry
```
```bash
dnf install -y policycoreutils*
```
```bash
dnf clean all
dnf repolist all
```
```bash
systemctl status autofs
```
