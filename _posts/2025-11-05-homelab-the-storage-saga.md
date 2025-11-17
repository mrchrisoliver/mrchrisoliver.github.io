---
title: Homelab - The Storage Saga 
date: 2025-11-05 15:00:00 +0100
---

Before we jump in to the current storage issues, I will explain how I've run a setup for ~10 years.
My first "NAS" was a 2TB Apple Time Capsule, which I used to serve media files to Infuse on Apple TV and still runs until this day. 
I then expanded it by adding a 4TB HDD into the USB port on the Time Capsule, which I used for about 7 years.
My last used setup was the 4TB USB HDD plugged into a mini pc, where I hosted Jellyfin.

When I got the chance to "upgrade" my setup, I opted for a dual bay DAS over a NAS to omit the NAS software and worring about CPU/RAM etc. 
Anyway.. I bought a USB DAS.. After purchase, my research in forums tell me that USB is not a great option and can cause headaches. 

My current work-in-progress solution is using mdadm on Proxmox host to manage the RAID, I have OMV VM to manage network shares, then I connect to those network shares in VM/LXC.
So far I have a Jellyfin LXC connected using SMB/CIFS. I have connected it by 
```
sudo mount -t cifs //10.0.1.120/Media /mnt/media -o username=chris,password=password,vers-3.0
```
This seems to be the best solution so far, but time will tell. 

To make it persistant, mounting via fstab works.
```
//server/share  /mnt/mountpoint  cifs  username=user,password=pass,uid=1000,gid=1000  0  0
```
then you can use ```sudo mount -a``` to mount. 
