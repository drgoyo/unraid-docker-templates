Unraid Forum Support Thread Template
Subject: [SUPPORT] Riven & Zilean - Dual-Stack (IPv4/IPv6) NVMe Optimized Templates

Post Content:

[center][img]https://raw.githubusercontent.com/rivenmedia/riven/main/assets/riven-dark.png[/img][/center]

[size=5][b]Riven & Zilean Media Suite[/b][/size] High-performance, automated media management for Real-Debrid users, optimized for Unraid systems with NVMe storage and Dual-Stack networking.

[b]GitHub Repository:[/b] [Your GitHub Link Here] [b]Official Riven Wiki:[/b] [url]https://wiki.riven.stream/[/url] [b]Official Zilean Repo:[/b] [url]https://github.com/iPromKnight/zilean[/url]

[size=4][b]🚀 The "Must-Read" for Success[/b][/size]

[b]1. The VFS User Script (Mandatory)[/b] Riven requires the host mount point to be "shared." Unraid cannot do this via the GUI. You [b]MUST[/b] use the User Scripts plugin: [code] #!/bin/bash mkdir -p /mnt/vmdisk/realdebrid mount --make-shared /mnt/vmdisk [/code] Set this to [b]At Startup of Array[/b].

[b]2. Path Mapping (The Slave Mode)[/b] When mapping the [b]/library[/b] path in the Riven template, you [b]MUST[/b] click "Edit" and change the [b]Access Mode[/b] to [b]RW: Slave[/b]. If you don't, Plex will see an empty folder.

[b]3. RAM Usage (Zilean Indexing)[/b] Zilean is a beast on first run. It will download the DMM hashlists and consume [b]6-7GB of RAM[/b] for about an hour. Once indexed, it drops to a very low footprint.

[size=4][b]🌐 Networking Design[/b][/size] These templates use [b]macvlan (eth0)[/b] to give each service a dedicated IP. This prevents the "Broken Pipe" and "Time Out" errors common with internal Docker bridges.

[list] []Frontend: .232 []Backend: .233 []Database: .234 []Zilean: .235 [/list]

[size=4][b]❓ Frequently Asked Questions[/b][/size]

[b]Q: Why is my Riven container restarting?[/b] [i]A: It’s likely waiting for the database to be 100% ready. I have optimized the healthchecks, but on first boot, it may take 2-3 restart cycles to sync. You can also manually restart the backend once the DB is "Healthy."[/i]

[b]Q: Can I run this alongside a Windows VM?[/b] [i]A: Yes! These templates include optimized SHM sizes (2GB for Riven) to ensure the media engine stays in RAM even when your Windows VM is crunching data.[/i]

[b]Q: I don't see any files in Plex![/b] [i]A: Check two things: 1. Did you run the User Script? 2. Is the Riven library mapping set to RW: Slave?[/i]

[b]Support:[/b] Please post your [b]docker logs riven[/b] output if you encounter issues!