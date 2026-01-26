# 🚀 Riven Media Suite & Zilean: Unraid Optimized Templates

This repository provides native Unraid XML templates for **Riven** and **Zilean**, optimized for high-performance setups utilizing NVMe storage and Dual-Stack (IPv4/IPv6) networking.

---

## 📖 Official Documentation & Links

| Project | GitHub Repository | Documentation |
| --- | --- | --- |
| **Riven** | [RivenMedia/Riven](https://github.com/rivenmedia/riven) | [Riven Wiki](https://wiki.riven.stream/) |
| **Zilean** | [iPromKnight/Zilean](https://github.com/iPromKnight/zilean) | [Zilean Repo](https://github.com/iPromKnight/zilean#readme) |

---

## 🛠️ Mandatory Prerequisite: The VFS Mount Script

Riven uses a FUSE filesystem to mount your Real-Debrid library. Because Unraid's GUI cannot natively set the required **rshared** propagation on the host side, you **MUST** run this script before starting the Riven container.

1. Install the **User Scripts** plugin from Community Apps.
2. Create a new script named `riven-vfs-init`.
3. Paste the following code (replace `/mnt/vmdisk` with your actual drive path):
```bash
#!/bin/bash
# Create the mount point and set propagation to shared
mkdir -p /mnt/vmdisk/realdebrid
mount --make-shared /mnt/vmdisk

```


4. Set the schedule to **"At Startup of Array"** and run it once manually.

---

## 🔐 Initial Setup & Authentication (The "First Login" Fix)

Riven v1.x uses **Better Auth**, which is extremely strict about local IP security and user registration. To successfully log in for the first time:

### **1. Generate Secrets**

Visit [riven.tv/generator](https://riven.tv/generator) to generate your:

* **Backend API Key** (32-char Hex)
* **Auth Secret** (Base64 string)

### **2. Bypass "Signup Disabled"**

On a fresh install, no users exist. To create your account:

* Set `ENABLE_PLEX_SIGNUP` to `true` in the **Frontend** template.
* Set `RIVEN_ADMIN_USER` to your exact **Plex Username** (Case-Sensitive).

### **3. Fix "No Auth State Cookie" (Chrome/Edge)**

If accessing Riven via a local IP (`http://192.168.x.x`), Chrome will block secure cookies.

* **Firefox:** Works natively without extra steps.
* **Chrome/Edge:** Navigate to `chrome://flags/#unsafely-treat-insecure-origin-as-secure`.
* Enable the flag and add `http://192.168.31.232:3000` to the text area. Relaunch your browser.

---

## 📂 Storage & Path Guidelines

* **Appdata (Metadata/DB):** `/mnt/vmdisk/appdata/riven/` -> Recommended on NVMe for performance.
* **Media (VFS Mount):** `/mnt/vmdisk/realdebrid/` -> The virtual folder where content appears.
* **Access Mode:** You **MUST** set the `/library` path mapping to **RW: Slave** in the Riven template settings.

---

## 🌐 Networking: Dual-Stack Macvlan (eth0)

These templates use **Custom: eth0** to assign dedicated IP addresses, bypassing the Unraid bridge to eliminate "Broken Pipe" database errors.

* **Riven Frontend:** `192.168.31.232`
* **Riven Backend:** `192.168.31.233`
* **Riven DB:** `192.168.31.234`
* **Zilean:** `192.168.31.235`

---

## ⚡ Performance & Troubleshooting

### **Zilean RAM Usage**

During the first 60 minutes, Zilean downloads massive hashlists. RAM usage will spike to **6-8GB**. This is normal. It will drop significantly once indexing is complete.

### **Ryzen/Intel Crash Fix**

These templates include the `--security-opt seccomp=unconfined` parameter to prevent the "Exit 139" Segfaults common on Ryzen and newer Intel CPUs.

### **Permission Denied**

If logs show permission errors, run this in your Unraid terminal:

```bash
chown -R 99:100 /mnt/vmdisk/appdata/riven

```

---

## 🧩 How to Install

1. **Add Repository:** Copy the URL of this GitHub repository.
2. **Community Apps:** Go to **Apps** > **Settings** > **Additional Repositories** and add the URL.
3. **Order of Deployment:**
* Start `riven-db` and `zilean` first.
* Wait for Zilean to finish indexing (check logs).
* Start `riven-backend`, then `riven-frontend`.