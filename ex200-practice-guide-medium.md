# RHCL / RHCSA EX200 Practice Questions (RHEL 10)

If you are preparing for EX200 and want simple, practical revision material, this guide is for you.

Each section is independent, so you can revise fast even when you only have 10-15 minutes.

---

## 1) Network and Hostname

```bash
nmcli connection show
ip a
nmcli device
nmcli connection modify <profile> ipv4.addresses <ip/prefix> ipv4.gateway <gateway> ipv4.dns <dns> ipv4.method manual
nmcli connection up <profile>
hostnamectl set-hostname <hostname>
```

---

## 2) Repository Configuration (BaseOS/AppStream)

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

---

## 3) HTTP on Port 82

```bash
dnf install -y httpd
systemctl enable --now httpd
semanage port -a -t http_port_t -p tcp 82
firewall-cmd --permanent --add-port=82/tcp
firewall-cmd --reload
curl http://localhost:82
```

---

## 4) Users and Groups

```bash
groupadd admin
useradd -G admin harry
useradd -G admin natasha
useradd -s /sbin/nologin sarah
passwd harry
passwd natasha
passwd sarah
```

---

## 5) Shared Directory with SGID

```bash
mkdir -p /common/admin
chgrp admin /common/admin
chmod 770 /common/admin
chmod g+s /common/admin
```

---

## 6) NFS Auto-mount with Timeout

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

---

## 7) Cron + Deny User

Crontab:
```cron
30 12 * * * /bin/echo "EX200"
```

Deny:
```bash
echo natasha >> /etc/cron.deny
```

---

## 8) ACL Task

```bash
cp /etc/fstab /var/tmp/
setfacl -m u:harry:rw /var/tmp/fstab
setfacl -m u:natasha:--- /var/tmp/fstab
getfacl /var/tmp/fstab
```

---

## 9) NTP / Chrony

```bash
dnf install -y chrony
systemctl enable --now chronyd
systemctl restart chronyd
chronyc sources
timedatectl
```

---

## 10) Find Files >4MB and Copy

```bash
mkdir -p /find/largefiles
find /etc -type f -size +4M -exec cp {} /find/largefiles/ \;
```

---

## 11) User with UID

```bash
useradd -u 6969 billy
passwd billy
```

---

## 12) Archive and Compress

```bash
tar -zcvf /root/ex200.tar.gz /var/tmp
```

---

## 13) UMASK

```bash
su - natasha
umask 0277
```

---

## 14) Password Expiry for New Users

`/etc/login.defs`
```text
PASS_MAX_DAYS 20
```

---

## 15) Passwordless sudo for admin Group

```text
%admin ALL=(ALL) NOPASSWD: ALL
```

---

## 16) Script Practice

```bash
#!/bin/bash
find /usr/share -type f -size -1M -exec cp -a {} /root/find \;
```

---

## 17) Root Password Reset (Recovery)

```bash
passwd root
touch /.autorelabel
exec /sbin/init
```

---

## 18) 512MB Swap

```bash
lsblk
fdisk /dev/sdb
mkswap /dev/sdb1
swapon -a
```

---

## 19) LVM with Extents

```bash
pvcreate /dev/sdb2
vgcreate -s 8M datastore /dev/sdb2
lvcreate -l 50 -n database datastore
mkfs.ext4 /dev/datastore/database
```

---

## 21) Extend LV by 100 Extents

```bash
lvextend -l +100 -r /dev/datastore/database
```

---

## 22) tuned Profile

```bash
dnf install -y tuned
systemctl enable --now tuned
tuned-adm recommend
tuned-adm active
```

---

## 24) systemd Timer

```ini
[Timer]
OnCalendar=hourly
Persistent=true
```

```bash
systemctl daemon-reload
systemctl enable --now testtimer.timer
```

---

## 24) Flatpak Remote

```bash
dnf install -y flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

---

## 25) Script Argument Order

```bash
#!/bin/bash
if [ $# -eq 2 ]; then
  echo "$2 $1"
else
  echo "Usage: $0 argument1 argument2"
  exit 1
fi
```

---

## 26) Default File for New Users

```bash
touch /etc/skel/Congrats
chmod 644 /etc/skel/Congrats
```

---

## 27) Password Policy

`/etc/login.defs`
```text
PASS_MAX_DAYS 90
```

`/etc/security/pwquality.conf`
```text
minlen = 8
```

---

## 28) Passwordless Root SSH (Lab Scenario)

```bash
ssh-keygen
ssh-copy-id root@serverb
ssh root@serverb
```

---

## Quick Revision Checklist

- Network and hostname
- Repositories
- SELinux + firewall
- Users / groups / sudo
- ACL / permissions / umask
- Cron / NFS / autofs
- Time sync (chrony)
- LVM / swap
- systemd timer
- Flatpak
- SSH key login

If you want the full detailed explanations, use the detailed version:
**`ex200-practice-guide.md`**
