---
title: Homelab - Install Tailscale on Proxmox LXC 
date: 2025-11-02 15:00:00 +0100
---

This is going to be a little reference of installing Tailscale on Proxmox LXC with Debian OS.

I used a Debian 12 - Bookworm for the LXC base OS.
I followed these steps:
```
https://tailscale.com/kb/1174/install-debian-bookworm
```
This didn't work to start Tailscale due to network contraints. 
The LXC had to be powered down, and the conf file edited.
```
nano /etc/pve/lxc/101.conf
```
Make sure features is configured like so:
```
features: keyctl=1,nesting=1
```
```
# Allow TUN device for Tailscale
lxc.cgroup2.devices.allow = c 10:200 rwm
lxc.mount.entry = /dev/net/tun dev/net/tun none bind,create=file
```
The LXC is started again and Tailscale can be ran, you will have to login/have an Tailscale account. 
```
sudo tailscale up --advertise-exit-node --advertise-routes=192.168.1.0/24
```
This showed errors as the LXC isn't forwarding ports. Changing these in LXC made it work:
```
sysctl -w net.ipv4.ip_forward=1
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```
And persist it in
```
nano /etc/sysctl.conf
```
```
net.ipv4.ip_forward=1
```

Now it works, and you can access Proxmox via LXC!
