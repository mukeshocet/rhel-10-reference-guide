# RHCL / RHCSA EX200 Practice Questions (RHEL 10 Style)

## Easy, Step-by-Step Revision Guide

> This guide is designed for quick practice.
> Each question is independent, so you can revise one topic at a time.

---

## Quick Start (5 Minutes)

1. Open two terminals:
   - Terminal 1: run commands
   - Terminal 2: run verification commands
2. Pick one question from the table of contents below.
3. Complete only that question.
4. Validate immediately using that question's **Verify** block.
5. If verification fails, use the **Common Mistakes** section.

---

## Table of Contents

- [Before You Start](#before-you-start)
- [Placeholder Rules (Use Consistent Values)](#placeholder-rules-use-consistent-values)
- [Q1) Configure Network and Hostname](#q1-configure-network-and-hostname)
- [Q2) Configure Repositories (HTTP / ISO / DVD)](#q2-configure-repositories-http--iso--dvd)
- [Q3) Make Existing HTTP Content Available on Port 82](#q3-make-existing-http-content-available-on-port-82)
- [Q4) Create Users and Group](#q4-create-users-and-group)
- [Q5) Shared Collaboration Directory with SGID](#q5-shared-collaboration-directory-with-sgid)
- [Q6) Auto-mount NFS Shares with Timeout](#q6-auto-mount-nfs-shares-with-timeout)
- [Q7) Cron Job and Deny One User](#q7-cron-job-and-deny-one-user)
- [Q8) ACL on `/var/tmp/fstab`](#q8-acl-on-vartmpfstab)
- [Q9) Configure NTP Client (Chrony)](#q9-configure-ntp-client-chrony)
- [Q10) Find Files Larger Than 4MB and Copy](#q10-find-files-larger-than-4mb-and-copy)
- [Q11) Create User with Specific UID](#q11-create-user-with-specific-uid)
- [Q12) Archive and Compress Directory](#q12-archive-and-compress-directory)
- [Q13) Set UMASK for User](#q13-set-umask-for-user)
- [Q14) Password Expiry Policy for New Users](#q14-password-expiry-policy-for-new-users)
- [Q15) Allow `admin` Group Passwordless sudo](#q15-allow-admin-group-passwordless-sudo)
- [Q16) Bash Script Practice](#q16-bash-script-practice)
- [Q17) Reset Forgotten Root Password](#q17-reset-forgotten-root-password)
- [Q18) Create 512MB Swap Partition](#q18-create-512mb-swap-partition)
- [Q19) Create LVM with Specific Extents](#q19-create-lvm-with-specific-extents)
- [Q21) Extend Existing LV by 100 Extents](#q21-extend-existing-lv-by-100-extents)
- [Q22) Enable Recommended tuned Profile](#q22-enable-recommended-tuned-profile)
- [Q24) Run Script with systemd Timer](#q24-run-script-with-systemd-timer)
- [Q24 (Flatpak) Configure Flatpak Remote](#q24-flatpak-configure-flatpak-remote)
- [Q25) Script That Prints Second Argument Then First](#q25-script-that-prints-second-argument-then-first)
- [Q26) Add File to All New Users’ Home Directories](#q26-add-file-to-all-new-users-home-directories)
- [Q27) Enforce Password Expiry and Minimum Length](#q27-enforce-password-expiry-and-minimum-length)
- [Q28) Passwordless Root SSH from ServerA to ServerB](#q28-passwordless-root-ssh-from-servera-to-serverb)
- [Quick Revision Checklist](#quick-revision-checklist)
- [Exam-Day Fast Checklist (Printable)](#exam-day-fast-checklist-printable)
- [Common Mistakes by Topic](#common-mistakes-by-topic)
- [FAQ](#faq)

---

## Before You Start

- Practice on lab VMs (for example: `servera.lab.example.com`, `serverb.lab.example.com`)
- Use root or `sudo` access where needed
- Read each question first, then run commands carefully
- In real exams, names, IPs, and passwords can be different

---

## Placeholder Rules (Use Consistent Values)

Use the same style everywhere to avoid confusion:

- `<username>`: `harry`, `natasha`, `sarah`
- `<profile>`: output of `nmcli connection show`
- `<ip/prefix>`: for example `192.168.1.150/24`
- `<gateway>`: for example `192.168.1.1`
- `<dns>`: for example `8.8.8.8`
- `<interface>`: output of `ip a` or `nmcli device`

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

### Verify
```bash
ip a
hostnamectl
ping -c 2 <gateway>
```

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

### Verify
```bash
dnf repolist
dnf info bash
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

### Verify
```bash
semanage port -l | grep http_port_t
firewall-cmd --list-ports
curl http://localhost:82
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

### Verify
```bash
id harry
id natasha
getent passwd sarah
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

### Verify
```bash
ls -ld /common/admin
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

### Verify
```bash
ls /automount/public
ls /automount/private
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

### Verify
```bash
crontab -l
grep -w natasha /etc/cron.deny
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

### Verify
```bash
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

### Verify
```bash
chronyc tracking
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

### Verify
```bash
ls -lh /find/largefiles
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

### Verify
```bash
id billy
```

---

## Q12) Archive and Compress Directory

### Goal
Backup `/var/tmp` to `/root/ex200.tar.gz`.

### Command
```bash
tar -zcvf /root/ex200.tar.gz /var/tmp
```

### Verify
```bash
tar -tzf /root/ex200.tar.gz | head
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

### Verify
```bash
umask
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

### Verify
```bash
chage -l test
```

---

## Q15) Allow `admin` Group Passwordless sudo

### Add via `visudo`
```text
%admin ALL=(ALL) NOPASSWD: ALL
```

### Verify
```bash
sudo -l
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

### Verify
```bash
ls -l /root/find | head
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

### Verify
- Reboot and confirm login works with new root password.

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

### Verify
```bash
swapon --show
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

### Verify
```bash
vgs
lvs
df -h | grep /mnt/database
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

### Verify
```bash
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

### Verify
```bash
systemctl status testtimer.timer
systemctl list-timers | grep testtimer
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

### Verify
```bash
flatpak remotes
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

### Verify
```bash
/script.sh alpha beta
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

### Verify
```bash
ls -l /home/testuser/Congrats
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

### Verify
```bash
grep -E "PASS_MAX_DAYS|minlen" /etc/login.defs /etc/security/pwquality.conf
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

## Exam-Day Fast Checklist (Printable)

- [ ] Read question fully once before typing commands
- [ ] Use placeholders carefully (`<username>`, `<interface>`, `<profile>`)
- [ ] Configure first, verify second
- [ ] Save persistent configs (`/etc/fstab`, repo files, systemd units)
- [ ] Reload/restart services after config changes
- [ ] Re-check with one final verification command

---

## Common Mistakes by Topic

- **Network**: wrong connection profile name in `nmcli connection modify`
- **Repos**: typo in `baseurl` or repo file not enabled
- **SELinux/Firewall**: port opened in firewall but not labeled with `semanage`
- **Users/Groups**: group name mismatch (`admin` vs `admins`)
- **ACL**: checking with `ls -l` instead of `getfacl`
- **LVM/Swap**: forgot `/etc/fstab` entry for persistence
- **systemd timer**: created unit files but forgot `systemctl daemon-reload`

---

## FAQ

### `semanage: command not found`
Install:
```bash
dnf install -y policycoreutils*
```

### Repo still not visible in `dnf repolist`
- Check `.repo` file syntax and `enabled=1`
- Run:
```bash
dnf clean all
dnf repolist all
```

### `autofs` not mounting
- Confirm service state:
```bash
systemctl status autofs
```
- Re-check `/etc/auto.master` and map file path.

---

## Final Note

Practice commands and verification together.
In EX200-style exams, marks are usually for working state, not only typing commands.
