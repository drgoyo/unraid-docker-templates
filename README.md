# 🚀 Riven Media Suite & Zilean: Unraid Optimized Templates

This repository provides native Unraid XML templates for **Riven** and **Zilean**, optimized for high-performance setups utilizing NVMe storage and Dual-Stack (IPv4/IPv6) networking.

---

## 📖 Official Documentation & Links

| Project | GitHub Repository | Documentation |
| :--- | :--- | :--- |
| **Riven** | [RivenMedia/Riven](https://github.com/rivenmedia/riven) | [Riven Wiki](https://wiki.riven.stream/) |
| **Zilean** | [iPromKnight/Zilean](https://github.com/iPromKnight/zilean) | [Zilean Repo](https://github.com/iPromKnight/zilean#readme) |

---

## 🛠️ Mandatory Prerequisite: The VFS Mount Script

Riven uses a FUSE filesystem to mount your Real-Debrid library. Because Unraid's GUI cannot natively set the required **rshared** propagation on the host side, you **MUST** run this script before starting the Riven container.

1. Install the **User Scripts** plugin from Community Apps.
2. Create a new script named `riven-vfs-init`.
3. Paste the following code:
   ```bash
   #!/bin/bash
   # Create the mount point and set propagation to shared
   mkdir -p /mnt/vmdisk/realdebrid
   mount --make-shared /mnt/vmdisk
Set the schedule to "At Startup of Array" and run it once manually.

📂 Storage & Path Guidelines (Best Practices)
To ensure stability and prevent recursive folder loops, these templates enforce a strict separation between Config (Appdata) and Media (VFS Mount).

Appdata (Metadata/DB): /mnt/vmdisk/appdata/riven/ -> Fast NVMe access for database snappiness.

Media (VFS Mount): /mnt/vmdisk/realdebrid/ -> The virtual folder where your content appears.

[!CAUTION] Access Mode: In the Riven container settings, ensure the mapping for /library is set to RW: Slave. If set to standard Read/Write, the files will not be visible to other containers like Plex.

🌐 Networking: Dual-Stack Macvlan (eth0)
These templates use Custom: eth0 to assign dedicated IP addresses. This bypasses the Unraid bridge and eliminates the "Broken Pipe" database errors common in complex Docker setups.

Riven Frontend: 192.168.31.232 | [fdcf:bbfb:5598::232]

Riven Backend: 192.168.31.233 | [fdcf:bbfb:5598::233]

Riven DB: 192.168.31.234 | [fdcf:bbfb:5598::234]

Zilean: 192.168.31.235 | [fdcf:bbfb:5598::235]

⚡ Performance & RAM Optimization
SHM (Shared Memory): Riven is pre-configured with 2GB SHM and the Database with 1GB SHM. This protects the metadata engine from crashes, especially on systems running heavy VMs (like Windows 11).

Zilean Indexing: On the first run, Zilean will download and index the DMM hashlists. Expect RAM usage to spike to 6-7GB for approximately 30-60 minutes. This is normal behavior.

🧩 How to Install
Add Repository: Copy the URL of this GitHub repository.

Community Apps: Go to the Apps tab in Unraid, click Settings, and add this URL to your "Additional Repositories".

Deploy Order: * Start riven-db and zilean first.

Wait for Zilean to finish its initial indexing.

Start riven (Backend) and then riven-frontend.