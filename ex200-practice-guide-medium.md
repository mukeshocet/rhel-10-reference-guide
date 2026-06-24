# RHCL / RHCSA EX200 Practice Questions (RHEL 10)

If you are preparing for EX200 and want simple, practical revision material, this guide is for you.

Each section is independent, so you can revise fast even when you only have 10-15 minutes.

---

## Quick Start

1. Pick one section.
2. Run commands in a lab VM.
3. Validate result before moving to next section.

**Start Practice:**

[![Beginner Set](https://img.shields.io/badge/Start-Beginner_Set-success)](#4-users-and-groups)
[![Exam-Critical Set](https://img.shields.io/badge/Start-Exam--Critical_Set-important)](#2-repository-configuration-baseosappstream)
[![1-Page Cheatsheet](https://img.shields.io/badge/Open-Cheatsheet-blue)](./ex200-one-page-cheatsheet.md)

---

## Jump to Topic

- [Question 1: Network and Hostname](#question-1-network-and-hostname)
- [Question 2: Repository Configuration (BaseOS/AppStream)](#question-2-repository-configuration-baseos-appstream)
- [Question 3: HTTP on Port 82](#question-3-http-on-port-82)
- [Question 4: Users and Groups](#question-4-users-and-groups)
- [Question 5: Shared Directory with SGID](#question-5-shared-directory-with-sgid)
- [Question 6: NFS Auto-mount with Timeout](#question-6-nfs-auto-mount-with-timeout)
- [Question 7: Cron + Deny User](#question-7-cron-deny-user)
- [Question 8: ACL Task](#question-8-acl-task)
- [Question 9: NTP / Chrony](#question-9-ntp-chrony)
- [Question 10: Find Files >4MB and Copy](#question-10-find-files-4mb-and-copy)
- [Question 11: User with UID](#question-11-user-with-uid)
- [Question 12: Archive and Compress](#question-12-archive-and-compress)
- [Question 13: UMASK](#question-13-umask)
- [Question 14: Password Expiry for New Users](#question-14-password-expiry-for-new-users)
- [Question 15: Passwordless sudo for admin Group](#question-15-passwordless-sudo-for-admin-group)
- [Question 16: Script Practice](#question-16-script-practice)
- [Question 17: Root Password Reset (Recovery)](#question-17-root-password-reset-recovery)
- [Question 18: 512MB Swap](#question-18-512mb-swap)
- [Question 19: LVM with Extents](#question-19-lvm-with-extents)
- [Question 21: Extend LV by 100 Extents](#question-21-extend-lv-by-100-extents)
- [Question 22: tuned Profile](#question-22-tuned-profile)
- [Question 24: systemd Timer](#question-24-systemd-timer)
- [Question 25: Flatpak Remote](#question-25-flatpak-remote)
- [Question 26: Script Argument Order](#question-26-script-argument-order)
- [Question 27: Default File for New Users](#question-27-default-file-for-new-users)
- [Question 28: Password Policy](#question-28-password-policy)
- [Question 29: Passwordless Root SSH (Lab Scenario)](#question-29-passwordless-root-ssh-lab-scenario)
- [RHEL 9 Only) Containers with Podman](#rhel-9-only-containers-with-podman)
- [Quick Revision Checklist](#quick-revision-checklist)

---

## Badge Legend

- `🟢` Beginner
- `🟡` Intermediate
- `🔴` Exam-Critical
- `⏱️` Estimated practice time

## Fast Practice Planner

| Topic Group | Difficulty | Time |
| --- | --- | --- |
| Network/Repos/Web (Question 1-Question 3) | 🔴 | ⏱️ 30-45 min |
| Users/Permissions/ACL (Question 4-Question 8) | 🔴 | ⏱️ 30-40 min |
| Time/Find/Archive/Policy (Question 9-Question 15) | 🟡 | ⏱️ 35-50 min |
| Scripting/Recovery/Storage (Question 16-Question 21) | 🔴 | ⏱️ 45-60 min |
| tuned/systemd/flatpak/SSH (Question 22-Question 29) | 🟡 | ⏱️ 35-45 min |

---

## Practice Mode A/B/C

### Mode A (15-20 min)
- 1 beginner section
- 1 verification check

### Mode B (35-45 min)
- 2 exam-critical sections
- quick review of common mistakes

### Mode C (60-90 min)
- 4-6 mixed sections as mini mock
- one final pass through the cheatsheet

---

## Question 1: Network and Hostname
⬆️ [Back to top](#jump-to-topic)

Recommended: use `nmtui` to configure networking with the text UI.

```bash
nmtui
```

Use the commands below to find the connected device or configure networking from the command line with `nmcli` when required:
Exam tip: you may see non-connected devices. Run `nmcli device` to find the connected interface and configure networking on that interface.

```bash
nmcli connection show
ip a
nmcli device
nmcli connection modify <profile> ipv4.addresses <ip/prefix> ipv4.gateway <gateway> ipv4.dns <dns> ipv4.method manual
nmcli connection up <profile>
hostnamectl set-hostname <hostname>
```

---

## Question 2: Repository Configuration (BaseOS/AppStream)
⬆️ [Back to top](#jump-to-topic)

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

## Question 3: HTTP on Port 82
⬆️ [Back to top](#jump-to-topic)

```bash
dnf install -y httpd
systemctl enable --now httpd
semanage port -a -t http_port_t -p tcp 82
firewall-cmd --permanent --add-port=82/tcp
firewall-cmd --reload
curl http://localhost:82
```

---

## Question 4: Users and Groups
⬆️ [Back to top](#jump-to-topic)

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

## Question 5: Shared Directory with SGID
⬆️ [Back to top](#jump-to-topic)

```bash
mkdir -p /common/admin
chgrp admin /common/admin
chmod 770 /common/admin
chmod g+s /common/admin
```

---

## Question 6: NFS Auto-mount with Timeout
⬆️ [Back to top](#jump-to-topic)

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

## Question 7: Cron + Deny User
⬆️ [Back to top](#jump-to-topic)

Crontab:
```cron
30 12 * * * /bin/echo "EX200"
```

Deny:
```bash
echo natasha >> /etc/cron.deny
```

---

## Question 8: ACL Task
⬆️ [Back to top](#jump-to-topic)

```bash
cp /etc/fstab /var/tmp/
setfacl -m u:harry:rw /var/tmp/fstab
setfacl -m u:natasha:--- /var/tmp/fstab
getfacl /var/tmp/fstab
```

---

## Question 9: NTP / Chrony
⬆️ [Back to top](#jump-to-topic)

```bash
dnf install -y chrony
systemctl enable --now chronyd
systemctl restart chronyd
chronyc sources
timedatectl
```

---

## Question 10: Find Files >4MB and Copy
⬆️ [Back to top](#jump-to-topic)

```bash
mkdir -p /find/largefiles
find /etc -type f -size +4M -exec cp {} /find/largefiles/ \;
```

---

## Question 11: User with UID
⬆️ [Back to top](#jump-to-topic)

```bash
useradd -u 6969 billy
passwd billy
```

---

## Question 12: Archive and Compress
⬆️ [Back to top](#jump-to-topic)

```bash
# gzip
tar -zcvf /root/ex200.tar.gz /var/tmp

# bz2
tar -jcvf /root/ex200.tar.bz2 /var/tmp

# xz
tar -Jcvf /root/ex200.tar.xz /var/tmp
```

---

## Question 13: UMASK
⬆️ [Back to top](#jump-to-topic)

```bash
su - natasha
umask 0277
```

---

## Question 14: Password Expiry for New Users
⬆️ [Back to top](#jump-to-topic)

`/etc/login.defs`
```text
PASS_MAX_DAYS 20
```

---

## Question 15: Passwordless sudo for admin Group
⬆️ [Back to top](#jump-to-topic)

```text
%admin ALL=(ALL) NOPASSWD: ALL
```

Verify: log in as a user who is part of the `admin` group, then run:

```bash
sudo -l
```

---

## Question 16: Script Practice
⬆️ [Back to top](#jump-to-topic)

```bash
#!/bin/bash
find /usr/share -type f -size -1M -exec cp -a {} /root/find \;
```

---

## Question 17: Root Password Reset (Recovery)
⬆️ [Back to top](#jump-to-topic)

```bash
passwd root
touch /.autorelabel
exec /sbin/init
```

---

## Question 18: 512MB Swap
⬆️ [Back to top](#jump-to-topic)

```bash
lsblk
fdisk /dev/sdb
mkswap /dev/sdb1
swapon -a
```

---

## Question 19: LVM with Extents
⬆️ [Back to top](#jump-to-topic)

```bash
pvcreate /dev/sdb2
vgcreate -s 8M datastore /dev/sdb2
lvcreate -l 50 -n database datastore
mkfs.ext4 /dev/datastore/database
```

If the task provides a size (not extents), use `-L`:

```bash
lvcreate -L 2G -n database datastore
```

---

## Question 21: Extend LV by 100 Extents
⬆️ [Back to top](#jump-to-topic)

```bash
lvextend -l +100 -r /dev/datastore/database
```

---

## Question 22: tuned Profile
⬆️ [Back to top](#jump-to-topic)

```bash
dnf install -y tuned
systemctl enable --now tuned
tuned-adm recommend
tuned-adm active
```

---

## Question 24: systemd Timer
⬆️ [Back to top](#jump-to-topic)

Exam note: often you are told to install a lookup RPM first; it usually drops the required script in the target directory mentioned in the task.

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

## Question 25: Flatpak Remote
⬆️ [Back to top](#jump-to-topic)

```bash
dnf install -y flatpak

# System-wide remote
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# User-only remote
su - <username>
flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

---

## Question 26: Script Argument Order
⬆️ [Back to top](#jump-to-topic)

Optional: this exact pattern is unlikely to appear in the exam. Actual exam script tasks are generally easier.

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

## Question 27: Default File for New Users
⬆️ [Back to top](#jump-to-topic)

```bash
touch /etc/skel/Congrats
chmod 644 /etc/skel/Congrats
```

---

## Question 28: Password Policy
⬆️ [Back to top](#jump-to-topic)

`/etc/login.defs`
```text
PASS_MAX_DAYS 90
```

`/etc/security/pwquality.conf`
```text
minlen = 8
```

---

## Question 29: Passwordless Root SSH (Lab Scenario)
⬆️ [Back to top](#jump-to-topic)

```bash
ssh-keygen
ssh-copy-id root@serverb
ssh root@serverb
```

---

## RHEL 9 Only) Containers with Podman
⬆️ [Back to top](#jump-to-topic)

Container-based questions are common in RHEL 9 exam variants. For RHEL 10 preparation, treat this as optional.

```bash
dnf search podman
dnf install -y podman container-tools
podman login registry.lab.example.com --tls-verify=false
```

Run container as `harry`:
```bash
su - harry
podman run -d --name watch registry.access.redhat.com/ubi8/ubi
podman ps
```

Create image/tag and service:
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
- (RHEL 9 only) Podman container + user service

## Common Mistakes (Fast Read)

- Wrong network profile name in `nmcli`
- Repo URL typo or `enabled=0`
- Forgot SELinux port label when using non-default web port
- Forgot `daemon-reload` after creating systemd unit files
- Forgot persistence updates in `/etc/fstab`

## Mini FAQ (Collapsible)

<details>
<summary><strong>Q: `semanage` command is missing. What should I install?</strong></summary>

```bash
dnf install -y policycoreutils*
```
</details>

<details>
<summary><strong>Q: My repo is configured but `dnf` still does not show it.</strong></summary>

```bash
dnf clean all
dnf repolist all
```

Also re-check `baseurl` and `enabled=1`.
</details>

<details>
<summary><strong>Q: autofs does not mount NFS paths.</strong></summary>

```bash
systemctl status autofs
```

Then review `/etc/auto.master` and your map file path.
</details>

If you want the full detailed explanations, use the detailed version:
**`ex200-practice-guide.md`**
