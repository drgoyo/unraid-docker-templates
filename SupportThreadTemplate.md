I have updated your Unraid Forum Support Template to include the critical lessons we learned today—specifically the Chrome Flag fix, the Plex Signup logic, and the Key Generator.

This is now a comprehensive guide that should prevent 99% of common support questions.

Unraid Forum Support Thread Template
Subject: [SUPPORT] Riven & Zilean - Dual-Stack (IPv4/IPv6) NVMe Optimized Templates

Post Content:

[center][img]https://raw.githubusercontent.com/rivenmedia/riven/main/assets/riven-dark.png[/img][/center]

[size=5][b]Riven & Zilean Media Suite[/b][/size] High-performance, automated media management for Real-Debrid users, optimized for Unraid systems with NVMe storage and Dual-Stack networking.

[b]GitHub Repository:[/b] [url][Your GitHub Link Here][/url] [b]Official Riven Wiki:[/b] [url]https://wiki.riven.stream/[/url] [b]Official Zilean Repo:[/b] [url]https://github.com/iPromKnight/zilean[/url]

[size=4][b]🚀 The "Must-Read" for Success[/b][/size]

[b]1. The VFS User Script (Mandatory)[/b] Riven requires the host mount point to be "shared." Unraid cannot do this via the GUI. You [b]MUST[/b] use the User Scripts plugin: [code] #!/bin/bash

Replace /mnt/vmdisk with your actual NVMe/SSD mount point
mkdir -p /mnt/vmdisk/realdebrid mount --make-shared /mnt/vmdisk [/code] Set this to [b]At Startup of Array[/b].

[b]2. Initial Login & "signup_disabled" Fix[/b] Better Auth is strict. On your first run: [list] []Generate your keys at [url=https://riven.tv/generator]riven.tv/generator[/url]. []Set [b]ENABLE_PLEX_SIGNUP[/b] to [b]true[/b] in the Frontend template. [*]Enter your [b]Plex Username[/b] in the [b]RIVEN_ADMIN_USER[/b] field. [/list]

[b]3. Browser Security (The "No Auth Cookie" Fix)[/b] If you are using Chrome/Edge via HTTP, the browser will block authentication cookies. [b]Fix:[/b] Navigate to [i]chrome://flags/#unsafely-treat-insecure-origin-as-secure[/i], enable it, and add [b]http://192.168.31.232:3000[/b] to the list. Alternatively, use [b]Firefox[/b].

[b]4. Path Mapping (The Slave Mode)[/b] When mapping the [b]/library[/b] path in the Riven template, you [b]MUST[/b] click "Edit" and change the [b]Access Mode[/b] to [b]RW: Slave[/b].

[size=4][b]🌐 Networking Design[/b][/size] These templates use [b]macvlan (eth0)[/b] to give each service a dedicated IP. This prevents "Broken Pipe" and "Time Out" errors common with internal Docker bridges.

[list] []Frontend: .232 []Backend: .233 []Database: .234 []Zilean: .235 [/list]

[size=4][b]❓ Frequently Asked Questions[/b][/size]

[b]Q: Why is Zilean using so much RAM?[/b] [i]A: During the first 60 minutes, Zilean downloads massive DMM hashlists. RAM will spike to [b]6-7GB[/b]. This is normal; it will drop significantly once indexing is complete.[/i]

[b]Q: My Ryzen server keeps crashing Zilean![/b] [i]A: These templates include the [b]seccomp=unconfined[/i] fix in the Extra Parameters to prevent the 'Exit 139' segfaults common on Ryzen/Intel CPUs.[/b]

[b]Q: I get a "Permission Denied" error in the Riven logs.[/b] [i]A: Your appdata folder might have the wrong ownership. Run [code]chown -R 99:100 /mnt/vmdisk/appdata/riven[/code] in your Unraid terminal.[/i]

[b]Support:[/b] Please post your [b]docker logs riven-backend[/b] output if you encounter issues!