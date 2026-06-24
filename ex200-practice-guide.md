# RHCL / RHCSA EX200 Practice Questions (RHEL 10 Style)

## Easy, Step-by-Step Revision Guide

> This guide is designed for quick practice.
> Each question is independent, so you can revise one topic at a time.

---

## Quick Start (5 Minutes)

1. Pick one question from the table of contents below.
2. Complete only that question.
3. Validate immediately using that question's **Verify** block.
4. If verification fails, use the **Common Mistakes** section.

**Start Practice:**

[![Start with Beginner Tasks](https://img.shields.io/badge/Start-Beginner_Tasks-success)](#q4-create-users-and-group)
[![Start Exam-Critical Tasks](https://img.shields.io/badge/Start-Exam--Critical_Tasks-important)](#q2-configure-repositories-http--iso--dvd)
[![Open 1-Page Cheatsheet](https://img.shields.io/badge/Open-1--Page_Cheatsheet-blue)](./ex200-one-page-cheatsheet.md)

---

## Table of Contents

- [Before You Start](#before-you-start)
- [Placeholder Rules (Use Consistent Values)](#placeholder-rules-use-consistent-values)
- [Question 1: Configure Network and Hostname](#question-1-configure-network-and-hostname)
- [Question 2: Configure Repositories (HTTP / ISO / DVD)](#question-2-configure-repositories-http-iso-dvd)
- [Question 3: Make Existing HTTP Content Available on Port 82](#question-3-make-existing-http-content-available-on-port-82)
- [Question 4: Create Users and Group](#question-4-create-users-and-group)
- [Question 5: Shared Collaboration Directory with SGID](#question-5-shared-collaboration-directory-with-sgid)
- [Question 6: Auto-mount NFS Shares with Timeout](#question-6-auto-mount-nfs-shares-with-timeout)
- [Question 7: Cron Job and Deny One User](#question-7-cron-job-and-deny-one-user)
- [Question 8: ACL on `/var/tmp/fstab`](#question-8-acl-on-var-tmp-fstab)
- [Question 9: Configure NTP Client (Chrony)](#question-9-configure-ntp-client-chrony)
- [Question 10: Find Files Larger Than 4MB and Copy](#question-10-find-files-larger-than-4mb-and-copy)
- [Question 11: Create User with Specific UID](#question-11-create-user-with-specific-uid)
- [Question 12: Archive and Compress Directory](#question-12-archive-and-compress-directory)
- [Question 13: Set UMASK for User](#question-13-set-umask-for-user)
- [Question 14: Password Expiry Policy for New Users](#question-14-password-expiry-policy-for-new-users)
- [Question 15: Allow `admin` Group Passwordless sudo](#question-15-allow-admin-group-passwordless-sudo)
- [Question 16: Bash Script Practice](#question-16-bash-script-practice)
- [Question 17: Reset Forgotten Root Password](#question-17-reset-forgotten-root-password)
- [Question 18: Create 512MB Swap Partition](#question-18-create-512mb-swap-partition)
- [Question 19: Create LVM with Specific Extents](#question-19-create-lvm-with-specific-extents)
- [Question 21: Extend Existing LV by 100 Extents](#question-21-extend-existing-lv-by-100-extents)
- [Question 22: Enable Recommended tuned Profile](#question-22-enable-recommended-tuned-profile)
- [Question 24: Run Script with systemd Timer](#question-24-run-script-with-systemd-timer)
- [Question 25: Configure Flatpak Remote](#question-25-configure-flatpak-remote)
- [Question 26: Script That Prints Second Argument Then First](#question-26-script-that-prints-second-argument-then-first)
- [Question 27: Add File to All New Users’ Home Directories](#question-27-add-file-to-all-new-users-home-directories)
- [Question 28: Enforce Password Expiry and Minimum Length](#question-28-enforce-password-expiry-and-minimum-length)
- [Question 29: Passwordless Root SSH from ServerA to ServerB](#question-29-passwordless-root-ssh-from-servera-to-serverb)
- [RHEL 9 Only) Container-Based Questions (Podman)](#rhel-9-only-container-based-questions-podman)
- [Quick Revision Checklist](#quick-revision-checklist)
- [Exam-Day Fast Checklist (Printable)](#exam-day-fast-checklist-printable)
- [Common Mistakes by Topic](#common-mistakes-by-topic)
- [FAQ](#faq)

---

## Badge Legend

- `🟢 Beginner` = direct, low-risk tasks
- `🟡 Intermediate` = multi-step tasks or service config
- `🔴 Exam-Critical` = commonly tested, easy to lose marks if missed
- `⏱️` = estimated hands-on practice time

---

## Practice Planner (Difficulty + Time)

| Question | Topic | Difficulty | Time |
| --- | --- | --- | --- |
| Question 1 | Network + Hostname | 🟡 Intermediate | ⏱️ 10-15 min |
| Question 2 | Repositories | 🔴 Exam-Critical | ⏱️ 8-12 min |
| Question 3 | HTTP + SELinux + Firewall | 🔴 Exam-Critical | ⏱️ 12-18 min |
| Question 4 | Users + Groups | 🟢 Beginner | ⏱️ 8-10 min |
| Question 5 | Shared Directory (SGID) | 🔴 Exam-Critical | ⏱️ 8-12 min |
| Question 6 | NFS + autofs | 🔴 Exam-Critical | ⏱️ 15-20 min |
| Question 7 | Cron + deny | 🟢 Beginner | ⏱️ 5-8 min |
| Question 8 | ACL | 🔴 Exam-Critical | ⏱️ 8-12 min |
| Question 9 | Chrony/NTP | 🟡 Intermediate | ⏱️ 6-10 min |
| Question 10 | find + copy | 🟢 Beginner | ⏱️ 5-8 min |
| Question 11 | user with UID | 🟢 Beginner | ⏱️ 3-5 min |
| Question 12 | tar backup | 🟢 Beginner | ⏱️ 3-5 min |
| Question 13 | umask | 🟡 Intermediate | ⏱️ 5-8 min |
| Question 14 | password expiry | 🟡 Intermediate | ⏱️ 5-8 min |
| Question 15 | sudoers | 🔴 Exam-Critical | ⏱️ 5-8 min |
| Question 16 | shell script | 🟡 Intermediate | ⏱️ 8-12 min |
| Question 17 | root reset flow | 🔴 Exam-Critical | ⏱️ 10-15 min |
| Question 18 | swap partition | 🔴 Exam-Critical | ⏱️ 10-15 min |
| Question 19 | LVM create + mount | 🔴 Exam-Critical | ⏱️ 12-18 min |
| Question 21 | LVM extend | 🔴 Exam-Critical | ⏱️ 6-10 min |
| Question 22 | tuned | 🟢 Beginner | ⏱️ 4-6 min |
| Question 24 | systemd timer | 🟡 Intermediate | ⏱️ 10-15 min |
| Question 25 | flatpak remote | 🟢 Beginner | ⏱️ 4-6 min |
| Question 26 | argument script | 🟢 Beginner | ⏱️ 5-8 min |
| Question 27 | /etc/skel | 🟢 Beginner | ⏱️ 4-6 min |
| Question 28 | password policy | 🟡 Intermediate | ⏱️ 6-10 min |
| Question 29 | SSH key login | 🔴 Exam-Critical | ⏱️ 8-12 min |

---

## Practice Mode A/B/C

### Mode A - Daily Sprint (15-20 min)
- Pick 1 `🟢 Beginner` task
- Execute + Verify
- Review 1 line from **Common Mistakes**

### Mode B - Focus Block (35-45 min)
- Pick 2 `🔴 Exam-Critical` tasks
- Complete both with full verification
- Run the **Exam-Day Fast Checklist**

### Mode C - Mini Mock (60-90 min)
- Pick 4-6 mixed tasks (network, users, ACL/LVM, service)
- No notes while executing
- Validate all outcomes at the end

---

## Before You Start

- Practice on lab VMs (for example: `node1.domain.com`, `node2.domain.com`)
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

## Question 1: Configure Network and Hostname
⬆️ [Back to top](#table-of-contents)

### Goal
Configure network settings and hostname for your servers.

### Recommended method
Use `nmtui` for the fastest and most exam-friendly way to configure network settings with a text UI:

```bash
nmtui
```

### Helpful CLI commands (optional)
Use these commands to identify the connected device or configure from the command line with `nmcli` when needed:

Exam tip: some interfaces may appear as disconnected. Use `nmcli device` to identify the connected device so you configure networking on the correct interface.
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

## Question 2: Configure Repositories (HTTP / ISO / DVD)
⬆️ [Back to top](#table-of-contents)

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

## Question 3: Make Existing HTTP Content Available on Port 82
⬆️ [Back to top](#table-of-contents)

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

## Question 4: Create Users and Group
⬆️ [Back to top](#table-of-contents)

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

## Question 5: Shared Collaboration Directory with SGID
⬆️ [Back to top](#table-of-contents)

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

## Question 6: Auto-mount NFS Shares with Timeout
⬆️ [Back to top](#table-of-contents)

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

## Question 7: Cron Job and Deny One User
⬆️ [Back to top](#table-of-contents)

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

## Question 8: ACL on `/var/tmp/fstab`
⬆️ [Back to top](#table-of-contents)

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

## Question 9: Configure NTP Client (Chrony)
⬆️ [Back to top](#table-of-contents)

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

## Question 10: Find Files Larger Than 4MB and Copy
⬆️ [Back to top](#table-of-contents)

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

## Question 11: Create User with Specific UID
⬆️ [Back to top](#table-of-contents)

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

## Question 12: Archive and Compress Directory
⬆️ [Back to top](#table-of-contents)

### Goal
Backup `/var/tmp` using gzip, bz2, or xz compression.

### Commands
```bash
# gzip
tar -zcvf /root/ex200.tar.gz /var/tmp

# bz2
tar -jcvf /root/ex200.tar.bz2 /var/tmp

# xz
tar -Jcvf /root/ex200.tar.xz /var/tmp
```

### Verify
```bash
tar -tzf /root/ex200.tar.gz | head
tar -tjf /root/ex200.tar.bz2 | head
tar -tJf /root/ex200.tar.xz | head
```

---

## Question 13: Set UMASK for User
⬆️ [Back to top](#table-of-contents)

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

## Question 14: Password Expiry Policy for New Users
⬆️ [Back to top](#table-of-contents)

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

## Question 15: Allow `admin` Group Passwordless sudo
⬆️ [Back to top](#table-of-contents)

### Add via `visudo`
```text
%admin ALL=(ALL) NOPASSWD: ALL
```

### Verify
```bash
su - <user-in-admin-group>
sudo -l
```

---

## Question 16: Bash Script Practice
⬆️ [Back to top](#table-of-contents)

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

## Question 17: Reset Forgotten Root Password
⬆️ [Back to top](#table-of-contents)

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

## Question 18: Create 512MB Swap Partition
⬆️ [Back to top](#table-of-contents)

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

## Question 19: Create LVM with Specific Extents
⬆️ [Back to top](#table-of-contents)

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

### Size vs extent note
If the question gives LV **size** (for example `500M`, `2G`) instead of extents, use `-L` instead of `-l`.

Example:
```bash
lvcreate -L 2G -n database datastore
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

## Question 21: Extend Existing LV by 100 Extents
⬆️ [Back to top](#table-of-contents)

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

## Question 22: Enable Recommended tuned Profile
⬆️ [Back to top](#table-of-contents)

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

## Question 24: Run Script with systemd Timer
⬆️ [Back to top](#table-of-contents)

### Exam note
RHEL 10 only: this question is targeted for RHEL 10 exam preparation.

### Task note
In many exam variants, you may first be asked to install a lookup RPM. That package usually places the required lookup script(s) in the expected directory, so use the provided script path from the question.

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

## Question 25: Configure Flatpak Remote
⬆️ [Back to top](#table-of-contents)

### Exam note
RHEL 10 only: this question is targeted for RHEL 10 exam preparation.

### Commands
```bash
dnf install -y flatpak

# System-wide remote (available to all users)
flatpak remotes
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak remote-ls
flatpak list

# User-only remote (available only to that user)
su - <username>
flatpak remotes --user
flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak remote-ls --user
```

### Verify
```bash
flatpak remotes
su - <username> -c "flatpak remotes --user"
```

---

## Question 26: Script That Prints Second Argument Then First
⬆️ [Back to top](#table-of-contents)

### Exam note
Optional practice only. This exact script pattern is not expected in the exam; real exam scripting tasks are usually simpler.

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

## Question 27: Add File to All New Users’ Home Directories
⬆️ [Back to top](#table-of-contents)

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

## Question 28: Enforce Password Expiry and Minimum Length
⬆️ [Back to top](#table-of-contents)

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

## Question 29: Passwordless Root SSH from ServerA to ServerB
⬆️ [Back to top](#table-of-contents)

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

## RHEL 9 Only) Container-Based Questions (Podman)
⬆️ [Back to top](#table-of-contents)

### Scope note
Container-focused EX200 tasks are commonly reported in RHEL 9 exam variants. They are generally not expected in the same form for RHEL 10-focused practice.

### Base setup commands
```bash
dnf search podman
dnf install -y podman container-tools
podman login registry.lab.example.com --tls-verify=false
```

### Question A) Run a container from image URL as user `harry`
```bash
su - harry
podman run -d --name watch registry.access.redhat.com/ubi8/ubi
podman ps
```

Alternative image example:
```bash
podman run -d --name watch docker.io/library/nginx
```

### Question B) Create image/tag `watch` as user `harry`
```bash
su - harry
podman pull <IMAGE_URL>
podman tag <IMAGE_URL> watch
podman images
```

If the task requires build instead of pull:
```bash
podman build -t watch <build-context-path-or-url>
```

### Question C) Create user systemd service for the container
Create container from image/tag:
```bash
su - harry
podman create --name watch_c watch
```

Generate and install user unit:
```bash
podman generate systemd --name watch_c --files --new
mkdir -p ~/.config/systemd/user
mv container-watch_c.service ~/.config/systemd/user/
systemctl --user daemon-reload
systemctl --user enable --now container-watch_c.service
systemctl --user status container-watch_c.service
podman ps
```

Enable lingering so user service keeps running without active login:
```bash
sudo loginctl enable-linger harry
```

### Verify
```bash
podman ps
podman images
systemctl --user status container-watch_c.service
loginctl show-user harry | grep Linger
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
- [ ] (RHEL 9 only) podman container + user systemd service

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

<details>
<summary><strong>`semanage: command not found`</strong></summary>

Install:

```bash
dnf install -y policycoreutils*
```
</details>

<details>
<summary><strong>Repo still not visible in `dnf repolist`</strong></summary>

- Check `.repo` file syntax and `enabled=1`
- Run:

```bash
dnf clean all
dnf repolist all
```
</details>

<details>
<summary><strong>`autofs` not mounting</strong></summary>

- Confirm service state:

```bash
systemctl status autofs
```

- Re-check `/etc/auto.master` and map file path.
</details>

---

## Final Note

Practice commands and verification together.
In EX200-style exams, marks are usually for working state, not only typing commands.
