### **GitHub README.md for your Unraid Templates**

```markdown
# Unraid Templates: Riven & Zilean (Dual-Stack Optimized)

This repository contains Unraid XML templates for the Riven Media Suite and Zilean Indexer. These templates are optimized for high-performance setups (NVMe) and use Macvlan (eth0) to ensure network stability between services.

---

## 🚀 Prerequisite: The VFS Mount Script
Riven uses a FUSE filesystem to "mount" your Real-Debrid library. Unraid's GUI cannot natively set the required **rshared** propagation on the host side. **You must run this script before starting the Riven container.**

1. Install the **User Scripts** plugin from Community Apps.
2. Create a new script named `riven-vfs-init`.
3. Paste the following code:
   ```bash
   #!/bin/bash
   # Create the mount point and set propagation to shared
   mkdir -p /mnt/vmdisk/realdebrid
   mount --make-shared /mnt/vmdisk

```

4. Set the schedule to **"At Startup of Array"** and run it once manually.

---

## 📂 Folder Management (Best Practices)

These templates enforce a strict separation between your **Configuration** files and your **Media** files.

| Folder Type | Recommended Host Path | Description |
| --- | --- | --- |
| **Appdata** | `/mnt/vmdisk/appdata/riven/` | Where databases and metadata live. (High IO) |
| **Media (VFS)** | `/mnt/vmdisk/realdebrid/` | The virtual mount point for your movies/shows. |

> [!IMPORTANT]
> **Never** map your Riven `/library` (media) to the same folder as your `/config` (appdata). This will cause recursive loops and container crashes.

---

## 🛠️ Installation Guide

1. **Add this Repository:** Add the URL of this GitHub repo to your Unraid "Docker Repositories" or via Community Apps.
2. **Network Setup:** Ensure IPv6 is enabled in Unraid Settings if you plan to use the Dual-Stack config.
3. **Template Order:**
* Install **riven-db** first.
* Install **zilean** and let it index (may take 30-60 mins).
* Install **riven** and **riven-frontend**.


4. **Permissions:** Ensure the mapping for `/library` in Riven is set to **RW: Slave** in the Unraid "Edit" window.

---

## 🌐 Network Logic

These templates use **macvlan (eth0)** with static IPs.

* **Frontend:** `.232`
* **Backend:** `.233`
* **Database:** `.234`
* **Zilean:** `.235`