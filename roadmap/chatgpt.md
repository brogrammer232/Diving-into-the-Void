# 🗺️ VOID LINUX MASTERY ROADMAP

*From "just installed it" to "I could maintain this distro if I had to" — with OS development in mind.*

> This roadmap is tailored for deep system mastery with a long-term goal of building your own operating system. It doesn't just teach you how to *use* Void Linux, it shows you how to *understand* and eventually *replace* parts of it. If a tool vanished tomorrow, you'd know how to rebuild it.

---

## 🔰 STAGE 0: PHILOSOPHY

> Objective: Know what makes Void different and why you're using it.

* [ ] Understand Void's core philosophy:

  * Simplicity
  * No systemd
  * Rolling release
  * Source & binary flexibility

* [ ] Know the implications of `musl` vs `glibc`

* [ ] Understand why `runit` was chosen over other init systems

* [ ] Study the [Void Handbook](https://docs.voidlinux.org/)

---

## 🧱 STAGE 1: FOUNDATIONS — SYSTEM LAYOUT, INIT, AND PACKAGE CORE

### 🔌 A. INIT SYSTEM: `runit`

**Concepts:**

* What is `PID 1` and why it matters
* `runsvdir` manages service directories
* `runsv` supervises individual services

**Skills:**

* Manage services:

  * `sv status <service>`
  * `sv restart <service>`
  * `ln -s /etc/sv/<service> /var/service/` (enable)
  * `rm /var/service/<service>` (disable)
* Read logs from `/var/log/`

**Deep Dives:**

* Write a custom runit service with logging and restart logic
* Read `runsvdir` and `runsv` manpages
* Explore `/etc/sv/` service structure (run, log, finish)

**Stretch Goal:**

* Implement a toy `runsv` clone in C or shell

---

### 📦 B. PACKAGE MANAGEMENT: `xbps` and `xbps-src`

**Concepts:**

* What a package *is* (metadata + binary files + install scripts)
* Installed vs downloaded vs built packages
* How dependency resolution and verification work

**Commands:**

* `xbps-install`, `xbps-remove`, `xbps-query`, `xbps-reconfigure`
* Search: `xbps-query -Rs <term>`
* Ownership: `xbps-query -o /usr/bin/foo`
* File list: `xbps-query -f <pkg>`

**Internals:**

* Explore `/var/db/xbps/`
* Understand transaction logs and package metadata

**Packaging with xbps-src:**

* Setup `void-packages` and run `xbps-src binary-bootstrap`
* Build a simple package (e.g., hello-world)
* Modify a template and rebuild

**Stretch Goal:**

* Design a toy package manager in shell or Python

---

### 📁 C. FILESYSTEM HIERARCHY

**Concepts:**

* What lives where and why (`/etc`, `/usr/bin`, `/proc`, `/sys`, etc.)
* Difference between static and dynamic filesystems

**Practice:**

* Explore contents of `/proc` and `/sys`
* Use `tree`, `du`, `find`, `stat` to understand layouts
* Create and mount a loopback filesystem

**Stretch Goal:**

* Write a toy virtual filesystem using FUSE (Python or C)

---

### 🧪 D. KERNEL MESSAGES AND DEVICE PROBING

**Tools:**

* `dmesg`, `lsmod`, `modinfo`, `modprobe`, `insmod`, `rmmod`
* `lspci`, `lsusb`, `udevadm`, `dmidecode`

**Practice:**

* Observe `dmesg` from boot to login
* Insert/remove modules and observe behavior
* Write a basic kernel module that logs `printk()`

**Stretch Goal:**

* Rebuild the kernel with a custom log message or disabled module

---

## 🛠️ STAGE 2: CLI FLUENCY AND SCRIPTING

**Objective:** Be fluent in manipulating files, processes, and text.

**Core Tools:**

* `grep`, `sed`, `awk`, `cut`, `xargs`, `tr`, `sort`, `uniq`, `tee`
* `find`, `locate`, `stat`, `du`, `df`
* `ps`, `htop`, `kill`, `lsof`, `strace`, `time`

**Practice:**

* Write a log-parsing tool
* Build a service watchdog script
* Automate file backups with a daily cron job

**Stretch Goal:**

* Replace a small system utility with your own version

---

## 🌐 STAGE 3: NETWORKING

**Concepts:**

* Interfaces, IP addressing, DNS, routing, VPNs

**Tools:**

* `ip`, `ifconfig`, `ping`, `traceroute`, `dig`, `ss`, `iptables`/`nftables`, `curl`, `wget`, `scp`, `rsync`

**Practice:**

* Manually configure static IP with `ip`
* Use `iptables` to block or allow services
* SSH into and sync files between machines

**Stretch Goal:**

* Set up and script a WiFi auto-connector with `wpa_supplicant`

---

## 🧙 STAGE 4: GRAPHICS AND INPUT (LOW PRIORITY FOR OS DEV)

**Useful but not core for kernel dev — skim these unless you're targeting user-facing systems.**

* X11 architecture: `.xinitrc`, `.Xresources`, DMs
* i3/dwm tiling WMs and keybinding systems
* Fonts, DPI, keyboard layouts, locale configs

**Stretch Goal:**

* Build a minimal X session from TTY with a WM and terminal

---

## 🧠 STAGE 5: SYSTEM DEEP KNOWLEDGE

### 🧩 A. BOOT PROCESS

* Understand the full chain: BIOS/UEFI → Bootloader → Kernel → Init
* Explore `/boot`, GRUB config, `vmlinuz`, `initramfs`

**Practice:**

* Edit GRUB entries to test boot options
* Rebuild `initramfs` manually

**Stretch Goal:**

* Replace GRUB with Limine or systemd-boot

---

### 🔬 B. PROCFS, SYSFS, AND DEVICE MANAGEMENT

* Navigate `/proc` and `/sys`
* Understand `udev` rules and device naming
* Practice interacting with hardware through these interfaces

**Stretch Goal:**

* Write a udev rule that triggers a script on USB insert

---

### 🔧 C. BUILDING AND PATCHING THE KERNEL

* Download Linux kernel source
* Use `make menuconfig`, `make bzImage`, `make modules`
* Install and boot custom kernel
* Patch and rebuild

**Stretch Goal:**

* Add your own system call or modify syscall behavior

---

## ⚙️ STAGE 6: BUILD SYSTEM & SOURCE-LEVEL PACKAGING

### 🧰 A. `xbps-src` DEEP DIVE

* Understand templates: variables, `do_build()`, `do_install()`
* Customize and build packages
* Patch packages manually and rebuild

### 🛠 B. REPO MAINTENANCE

* Host your own repo
* Sign packages
* Create a custom metapackage

---

## 🧨 STAGE 7: SYSTEM HARDENING & BACKUPS

* File permissions, user groups, `sudo`
* SSH key authentication
* `rsync`, `borg`, `tar` for backups
* Log rotation and cleanup scripts

**Stretch Goal:**

* Write a weekly cron job that cleans old logs and backups

---

## 🧞 STAGE 8: CUSTOMIZATION (LOW PRIORITY FOR KERNEL/OS DEV)

* Custom prompts with `PS1`, `zsh`, `bashrc`
* Dotfiles repo
* Window manager status bars and launchers
* Clipboard utilities and hotkey daemons

---

## 🏁 FINAL BOSS: VOID ISO BUILDER

Build your own live ISO with:

* Custom kernel
* Your own WM or console environment
* Preinstalled dotfiles and tools
* Your custom runit services
* Optional: modified initramfs

---

## 🧭 META ADVICE

> If you can't fix your system from a TTY after X crashes, you don't own it yet.

> If you can explain how a binary is installed, launched, and scheduled by the kernel — you're getting close to writing your own OS.

> Read logs. Then read source. Then write your own.
