# RHCL / RHCSA EX200 Practice Questions (RHEL 10 Style)

## Easy, Step-by-Step Revision Guide

> This guide is designed for quick practice.
> Each question is independent, so you can revise one topic at a time.

---

## Before You Start

- Practice on lab VMs (for example: `servera.lab.example.com`, `serverb.lab.example.com`)
- Use root or `sudo` access where needed
- Read each question first, then run commands carefully
- In real exams, names, IPs, and passwords can be different

---

## Q1) Configure Network and Hostname

### Goal
Configure network settings and hostname for your servers.

### Useful commands
```bash
nmcli connection show
ip a
nmcli device
nmcli connection modify <profile> ipv4.addresses <ip/prefix> ipv4.gateway <gateway> ipv4.dns <dns> ipv4.method manual
nmcli connection up <profile>
hostnamectl set-hostname <hostname>
```

### Notes
- If device name is different (`enp0s3`, `ens...`), use your actual interface name.
- Network profiles are stored in:
`/etc/NetworkManager/system-connections/`

---

## Q2) Configure Repositories (HTTP / ISO / DVD)

### Goal
Set up BaseOS and AppStream repositories.

### Example `.repo` file
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

### ISO/DVD mounting options
```bash
mount -o loop RHEL-9.iso /mnt
mount /dev/sr0 /mnt
```

### Validation
```bash
dnf clean all
dnf repolist all
dnf search httpd
dnf install httpd
```

---

## Q3) Make Existing HTTP Content Available on Port 82

### Goal
HTTP content exists already. Do not modify content. Make it reachable on port 82.

### Commands
```bash
dnf install -y httpd
systemctl enable --now httpd
semanage port -a -t http_port_t -p tcp 82
firewall-cmd --permanent --add-port=82/tcp
firewall-cmd --reload
curl http://localhost:82
```

If `semanage` is missing:
```bash
dnf install -y policycoreutils*
```

---

## Q4) Create Users and Group

### Goal
Create:
- group `admin`
- user `harry` in `admin`
- user `natasha` in `admin`
- user `sarah` with non-interactive shell
- set passwords

### Commands
```bash
groupadd admin
useradd -G admin harry
useradd -G admin natasha
useradd -s /sbin/nologin sarah
passwd harry
passwd natasha
passwd sarah
```

Check:
```bash
id <username>
cat /etc/group
cat /etc/passwd
```

---

## Q5) Shared Collaboration Directory with SGID

### Goal
Create `/common/admin` with:
- group owner `admin`
- full access for owner/group only
- new files inherit group `admin`

### Commands
```bash
mkdir -p /common/admin
chgrp admin /common/admin
chmod 770 /common/admin
chmod g+s /common/admin
```

Test:
```bash
touch /common/admin/testfile
ls -l /common/admin
```

---

## Q6) Auto-mount NFS Shares with Timeout

### Goal
Auto-mount:
- `nfs.lab.example.com:/public` (read-only)
- `nfs.lab.example.com:/private` (read-write)
- unmount if idle for 30s

### Commands
```bash
dnf install -y nfs* autofs*
mkdir -p /automount/public /automount/private
```

`/etc/auto.master` entry:
```text
/automount /etc/auto.nfs --timeout=30
```

`/etc/auto.nfs`:
```text
public  -ro,sync  nfs.lab.example.com:/public
private -rw,sync  nfs.lab.example.com:/private
```

Start service:
```bash
systemctl enable --now autofs
```

Check:
```bash
showmount -e nfs.lab.example.com
df -h
```

---

## Q7) Cron Job and Deny One User

### Goal
- As `harry`, run job at `12:30`
- deny `natasha` from cron

### Commands
```bash
su - harry
crontab -e
```

Crontab line:
```cron
30 12 * * * /bin/echo "EX200"
```

Deny user:
```bash
echo natasha >> /etc/cron.deny
```

Check:
```bash
crontab -l
cat /etc/cron.deny
```

---

## Q8) ACL on `/var/tmp/fstab`

### Goal
- copy `/etc/fstab` to `/var/tmp`
- `harry` gets read/write
- `natasha` gets no access
- others can read (as required in question)

### Commands
```bash
cp /etc/fstab /var/tmp/
setfacl -m u:harry:rw /var/tmp/fstab
setfacl -m u:natasha:--- /var/tmp/fstab
getfacl /var/tmp/fstab
```

---

## Q9) Configure NTP Client (Chrony)

### Goal
Use `ntp.lab.example.com` as time source.

### Commands
```bash
dnf install -y chrony
```

In `/etc/chrony.conf`, keep:
```text
server ntp.lab.example.com iburst
```

Then:
```bash
systemctl enable --now chronyd
systemctl restart chronyd
chronyc sources
timedatectl
```

---

## Q10) Find Files Larger Than 4MB and Copy

### Goal
Find files in `/etc` greater than `4MB` and copy to `/find/largefiles`.

### Commands
```bash
mkdir -p /find/largefiles
find /etc -type f -size +4M -exec cp {} /find/largefiles/ \;
ls /find/largefiles
```

---

## Q11) Create User with Specific UID

### Goal
Create user `billy` with UID `6969`.

### Commands
```bash
useradd -u 6969 billy
passwd billy
cat /etc/passwd | grep billy
```

---

## Q12) Archive and Compress Directory

### Goal
Backup `/var/tmp` to `/root/ex200.tar.gz`.

### Command
```bash
tar -zcvf /root/ex200.tar.gz /var/tmp
```

---

## Q13) Set UMASK for User

### Goal
Set default permissions for `natasha` using umask.

### Commands
```bash
su - natasha
umask 0277
```

Persistent:
Add to `~/.bash_profile`:
```bash
umask 0277
```

---

## Q14) Password Expiry Policy for New Users

### Goal
Set max password age (example in source: `20` days).

### In `/etc/login.defs`
```text
PASS_MAX_DAYS 20
```

Test:
```bash
useradd test
chage -l test
```

---

## Q15) Allow `admin` Group Passwordless sudo

### Add via `visudo`
```text
%admin ALL=(ALL) NOPASSWD: ALL
```

---

## Q16) Bash Script Practice

### Goal
Create script to find files smaller than `1M` in `/usr/share` and copy to `/root/find`.

### Commands
```bash
mkdir -p /root/find
```

Script (`gofind.sh`):
```bash
#!/bin/bash
find /usr/share -type f -size -1M -exec cp -a {} /root/find \;
```

Enable/Run:
```bash
chmod +x gofind.sh
./gofind.sh
```

---

## Q17) Reset Forgotten Root Password

### High-level flow
1. Reboot and edit GRUB entry (`e`)
2. On `linux` line, set `rw` and append `init=/bin/bash`
3. Boot with `Ctrl+x`
4. Reset password:

```bash
passwd root
touch /.autorelabel
exec /sbin/init
```

---

## Q18) Create 512MB Swap Partition

### Commands (example disk)
```bash
lsblk
fdisk /dev/sdb
mkswap /dev/sdb1
blkid /dev/sdb1
```

Add to `/etc/fstab`:
```text
UUID=<uuid> swap swap defaults 0 0
```

Enable:
```bash
swapon -a
swapon -s
```

---

## Q19) Create LVM with Specific Extents

### Goal (as source style)
- VG with extent size 8MiB
- LV in that VG with 50 extents
- format and mount persistently

### Commands (example)
```bash
pvcreate /dev/sdb2
vgcreate -s 8M datastore /dev/sdb2
lvcreate -l 50 -n database datastore
mkfs.ext4 /dev/datastore/database
mkdir -p /mnt/database
```

`/etc/fstab` entry example:
```text
/dev/datastore/database /mnt/database ext4 defaults 0 0
```

Mount:
```bash
mount -a
```

---

## Q21) Extend Existing LV by 100 Extents

### Command
```bash
lvextend -l +100 -r /dev/datastore/database
```

Verify:
```bash
lvs
lvdisplay
```

---

## Q22) Enable Recommended tuned Profile

### Commands
```bash
dnf install -y tuned
systemctl enable --now tuned
tuned-adm recommend
tuned-adm profile <recommended-profile>
tuned-adm active
```

---

## Q24) Run Script with systemd Timer

### Script
`/usr/local/testtimer.sh`
```bash
#!/bin/bash
echo hello >> /var/log/examscript.log
```

```bash
chmod +x /usr/local/testtimer.sh
```

### Service unit (`/etc/systemd/system/testtimer.service`)
```ini
[Unit]
Description=testtimer

[Service]
Type=oneshot
ExecStart=/usr/local/testtimer.sh
```

### Timer unit (`/etc/systemd/system/testtimer.timer`)
```ini
[Unit]
Description=Run testtimer script on schedule

[Timer]
OnCalendar=hourly
Persistent=true

[Install]
WantedBy=timers.target
```

### Enable
```bash
systemctl daemon-reload
systemctl enable --now testtimer.timer
systemctl list-timers
journalctl -u testtimer.service
```

---

## Q24 (Flatpak) Configure Flatpak Remote

### Commands
```bash
dnf install -y flatpak
flatpak remotes
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak remote-ls
flatpak list
```

---

## Q25) Script That Prints Second Argument Then First

### `/script.sh`
```bash
#!/bin/bash
if [ $# -eq 2 ]; then
  echo "$2 $1"
else
  echo "Usage: $0 argument1 argument2"
  exit 1
fi
```

```bash
chmod +x /script.sh
/script.sh test1 test2
```

Expected output:
```text
test2 test1
```

---

## Q26) Add File to All New Users’ Home Directories

### Commands
```bash
touch /etc/skel/Congrats
chmod 644 /etc/skel/Congrats
useradd testuser
ls /home/testuser
```

---

## Q27) Enforce Password Expiry and Minimum Length

### `/etc/login.defs`
```text
PASS_MAX_DAYS 90
```

### `/etc/security/pwquality.conf`
```text
minlen = 8
```

---

## Q28) Passwordless Root SSH from ServerA to ServerB

### On ServerB
```bash
dnf install -y openssh-server
systemctl enable --now sshd
```

In `/etc/ssh/sshd_config`:
```text
PermitRootLogin yes
```

```bash
systemctl restart sshd
```

### On ServerA
```bash
dnf install -y openssh-clients
ssh-keygen
ssh-copy-id root@serverb
ssh root@serverb
```

---

## Quick Revision Checklist

- [ ] Network and hostname
- [ ] Repo config (BaseOS/AppStream)
- [ ] SELinux port and firewall
- [ ] Users/groups/sudo
- [ ] ACL + permissions + umask
- [ ] Cron + autofs + NFS
- [ ] Chrony/NTP
- [ ] Find/copy/archive
- [ ] LVM/swap
- [ ] systemd timer
- [ ] flatpak remote
- [ ] SSH key-based access

---

## Final Note

Practice commands and verification together.
In EX200-style exams, marks are usually for working state, not only typing commands.
