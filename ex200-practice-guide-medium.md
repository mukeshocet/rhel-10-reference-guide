# RHCL / RHCSA EX200 Practice Questions (RHEL 10)

If you are preparing for EX200 and want simple, practical revision material, this guide is for you.

Each section is independent, so you can revise fast even when you only have 10-15 minutes.

---

## Quick Start

1. Pick one section.
2. Run commands in a lab VM.
3. Validate result before moving to next section.

---

## Jump to Topic

- [1) Network and Hostname](#1-network-and-hostname)
- [2) Repository Configuration (BaseOS/AppStream)](#2-repository-configuration-baseosappstream)
- [3) HTTP on Port 82](#3-http-on-port-82)
- [4) Users and Groups](#4-users-and-groups)
- [5) Shared Directory with SGID](#5-shared-directory-with-sgid)
- [6) NFS Auto-mount with Timeout](#6-nfs-auto-mount-with-timeout)
- [7) Cron + Deny User](#7-cron--deny-user)
- [8) ACL Task](#8-acl-task)
- [9) NTP / Chrony](#9-ntp--chrony)
- [10) Find Files >4MB and Copy](#10-find-files-4mb-and-copy)
- [11) User with UID](#11-user-with-uid)
- [12) Archive and Compress](#12-archive-and-compress)
- [13) UMASK](#13-umask)
- [14) Password Expiry for New Users](#14-password-expiry-for-new-users)
- [15) Passwordless sudo for admin Group](#15-passwordless-sudo-for-admin-group)
- [16) Script Practice](#16-script-practice)
- [17) Root Password Reset (Recovery)](#17-root-password-reset-recovery)
- [18) 512MB Swap](#18-512mb-swap)
- [19) LVM with Extents](#19-lvm-with-extents)
- [21) Extend LV by 100 Extents](#21-extend-lv-by-100-extents)
- [22) tuned Profile](#22-tuned-profile)
- [24) systemd Timer](#24-systemd-timer)
- [24) Flatpak Remote](#24-flatpak-remote)
- [25) Script Argument Order](#25-script-argument-order)
- [26) Default File for New Users](#26-default-file-for-new-users)
- [27) Password Policy](#27-password-policy)
- [28) Passwordless Root SSH (Lab Scenario)](#28-passwordless-root-ssh-lab-scenario)
- [Quick Revision Checklist](#quick-revision-checklist)

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

## Common Mistakes (Fast Read)

- Wrong network profile name in `nmcli`
- Repo URL typo or `enabled=0`
- Forgot SELinux port label when using non-default web port
- Forgot `daemon-reload` after creating systemd unit files
- Forgot persistence updates in `/etc/fstab`

If you want the full detailed explanations, use the detailed version:
**`ex200-practice-guide.md`**
