# EX200 One-Page Cheatsheet (RHEL 10 Style)

Use this page for fast revision before practice sessions.

> Quick links:
> [Detailed Guide](./ex200-practice-guide.md) | [Medium Guide](./ex200-practice-guide-medium.md) | [Home](./index.md)

---

## 1) Network + Hostname

```bash
nmcli connection show
ip a
nmcli device
nmcli connection modify <profile> ipv4.addresses <ip/prefix> ipv4.gateway <gateway> ipv4.dns <dns> ipv4.method manual
nmcli connection up <profile>
hostnamectl set-hostname <hostname>
```

## 2) Repositories

```ini
[BaseOS]
baseurl=http://repo.lab.example.com/rocky9.5/repo/BaseOS
enabled=1
gpgcheck=0

[AppStream]
baseurl=http://repo.lab.example.com/rocky9.5/repo/AppStream
enabled=1
gpgcheck=0
```

```bash
dnf clean all
dnf repolist all
```

## 3) HTTP on port 82

```bash
dnf install -y httpd
systemctl enable --now httpd
semanage port -a -t http_port_t -p tcp 82
firewall-cmd --permanent --add-port=82/tcp
firewall-cmd --reload
curl http://localhost:82
```

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

## 5) Shared Dir (SGID)

```bash
mkdir -p /common/admin
chgrp admin /common/admin
chmod 770 /common/admin
chmod g+s /common/admin
```

## 6) NFS + autofs

```bash
dnf install -y nfs* autofs*
systemctl enable --now autofs
```

`/etc/auto.master`
```text
/automount /etc/auto.nfs --timeout=30
```

`/etc/auto.nfs`
```text
public  -ro,sync  nfs.lab.example.com:/public
private -rw,sync  nfs.lab.example.com:/private
```

## 7) Cron + deny

```cron
30 12 * * * /bin/echo "EX200"
```

```bash
echo natasha >> /etc/cron.deny
```

## 8) ACL

```bash
cp /etc/fstab /var/tmp/
setfacl -m u:harry:rw /var/tmp/fstab
setfacl -m u:natasha:--- /var/tmp/fstab
getfacl /var/tmp/fstab
```

## 9) Chrony

```bash
dnf install -y chrony
systemctl enable --now chronyd
chronyc sources
timedatectl
```

## 10) Find >4MB

```bash
mkdir -p /find/largefiles
find /etc -type f -size +4M -exec cp {} /find/largefiles/ \;
```

## 11) User UID 6969

```bash
useradd -u 6969 billy
passwd billy
```

## 12) Archive

```bash
# gzip
tar -zcvf /root/ex200.tar.gz /var/tmp

# bz2
tar -jcvf /root/ex200.tar.bz2 /var/tmp

# xz
tar -Jcvf /root/ex200.tar.xz /var/tmp
```

## 13) umask

```bash
umask 0277
```

## 14) Password expiry

`/etc/login.defs`
```text
PASS_MAX_DAYS 20
```

## 15) Passwordless sudo

```text
%admin ALL=(ALL) NOPASSWD: ALL
```

Verify as an `admin` group user:

```bash
sudo -l
```

## 16) Script practice

```bash
find /usr/share -type f -size -1M -exec cp -a {} /root/find \;
```

## 17) Root reset flow

```bash
passwd root
touch /.autorelabel
exec /sbin/init
```

## 18) Swap 512MB

```bash
fdisk /dev/sdb
mkswap /dev/sdb1
swapon -a
```

## 19 + 21) LVM create + extend

```bash
pvcreate /dev/sdb2
vgcreate -s 8M datastore /dev/sdb2
lvcreate -l 50 -n database datastore
# If size is given instead of extents, use -L:
lvcreate -L 2G -n database datastore
mkfs.ext4 /dev/datastore/database
lvextend -l +100 -r /dev/datastore/database
```

## 22) tuned

```bash
dnf install -y tuned
systemctl enable --now tuned
tuned-adm recommend
tuned-adm active
```

## 24) systemd timer

Exam note: install lookup RPM first if instructed; it usually places the script in the required directory.

```bash
systemctl daemon-reload
systemctl enable --now testtimer.timer
systemctl list-timers
```

## 24) flatpak

```bash
dnf install -y flatpak

# System-wide remote
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# User-only remote
su - <username>
flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

## 25) reverse args script

Optional practice; exam scripts are usually easier and may not use this exact pattern.

```bash
if [ $# -eq 2 ]; then echo "$2 $1"; else echo "Usage: $0 argument1 argument2"; exit 1; fi
```

## 26) /etc/skel file

```bash
touch /etc/skel/Congrats
chmod 644 /etc/skel/Congrats
```

## 27) password policy

`/etc/security/pwquality.conf`
```text
minlen = 8
```

## 28) SSH key login

```bash
ssh-keygen
ssh-copy-id root@serverb
ssh root@serverb
```

## RHEL 9 only) Podman container tasks (optional for RHEL 10 prep)

```bash
dnf search podman
dnf install -y podman container-tools
podman login registry.lab.example.com --tls-verify=false
```

Run as user `harry`:
```bash
su - harry
podman run -d --name watch registry.access.redhat.com/ubi8/ubi
podman ps
```

Create user service:
```bash
podman create --name watch_c watch
podman generate systemd --name watch_c --files --new
mkdir -p ~/.config/systemd/user
mv container-watch_c.service ~/.config/systemd/user/
systemctl --user daemon-reload
systemctl --user enable --now container-watch_c.service
sudo loginctl enable-linger harry
```

---

## Final 60-Second Check

- [ ] services reloaded/restarted
- [ ] persistent files updated (`/etc/fstab`, repo, unit files)
- [ ] verification command run for each completed task
