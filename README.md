# networkwalks-B084-week1-cyber-security-lab-setup1
Cyber Security lab setup
 
# Week 1 | Project Module 1: Practical Lab Environment Setup

**Student:** Ahmad Muhammad
**Task:** Set up a cybersecurity testing lab on my laptop using VirtualBox and Kali Linux.

## Lab Overview

| Item | Value |
|---|---|
| Host OS | Windows 11 |
| Hypervisor | Oracle VirtualBox 7.2.22 |
| Attacker VM | Kali Linux 2026.3 (VirtualBox amd64 image) |
| VM Resources | 4096 MB RAM, 2 CPUs, 80 GB virtual disk |
| Network | NAT Network `NatNetwork`, `10.0.0.0/24`, DHCP enabled |
| Kali IP | `10.0.0.2/24`, gateway `10.0.0.1` |
| Shared folder | Windows `Downloads` folder |

## Requirements Checklist

- [x] VirtualBox used as base
- [x] Kali Linux set up as the attacking machine (as a VM)
- [x] NAT Network with the `10.0.0.0/24` subnet
- [x] Clipboard and drag/drop enabled (Bidirectional)
- [x] Shared folder enabled with the host Downloads folder
- [x] Kali IP address set to `10.0.0.2/24`
- [ ] Kali full internet access (in progress, see Known Issue)
- [ ] Snapshot of the VM

## Steps Followed

1. **Installed 7-Zip** from https://7-zip.org/download.html to extract the Kali archive.
2. **Installed VirtualBox 7.2.22** from https://virtualbox.org/wiki/Downloads. A "Missing Dependencies" warning about the optional Python bindings appeared, and I continued because they are not needed for this lab.
3. **Created the NAT Network:** File > Tools > Network (Ctrl+H) > NAT Networks > Create. Name `NatNetwork`, IPv4 Prefix `10.0.0.0/24` (changed from the default `10.0.2.0/24`), DHCP enabled, IPv6 disabled.
4. **Imported Kali Linux:** downloaded the VirtualBox 64-bit image from https://www.kali.org/get-kali/, extracted it with 7-Zip, and added the `.vbox` file with Machine > Add.
5. **Configured the VM settings:**
   - System: Base Memory 4096 MB
   - Network, Adapter 1: Attached to NAT Network, Name `NatNetwork`, Promiscuous Mode Allow All
   - General > Features: Shared Clipboard and Drag'n'Drop set to Bidirectional
   - Shared Folders: host Downloads folder added with Auto-mount and Full access
6. **Set the static IP in Kali** (Edit Connections > Wired connection 1 > IPv4 Settings > Manual):

   | Address | Netmask | Gateway | DNS |
   |---|---|---|---|
   | 10.0.0.2 | 24 | 10.0.0.1 | 8.8.8.8 |

## Known Issue: Internet Access

This is a known issue with VirtualBox 7 and Kali 2026.1 and newer. On testing, pinging `8.8.8.8` returned "Destination Host Unreachable" and the browser showed "Server Not Found". Fixes being applied:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

I am also confirming that Adapter 1 is attached to `NatNetwork` and not the default NAT, and that no other VM is using `10.0.0.2`.

## Problems I Ran Into

| Problem | Cause | Fix |
|---|---|---|
| Installer showed Repair / Remove | Same version already installed | Cancelled |
| `kali.org/get-kali,` gave a 404 | A trailing comma was in the URL | Typed the URL manually |
| Extracted files were `.rsrc` and `.data` | I extracted the VirtualBox installer by mistake | Extracted the correct Kali `.7z` |
| Create made a Host-only adapter | I was on the wrong tab | Switched to the NAT Networks tab |
| NAT Network showed `10.0.2.0/24` | VirtualBox default | Changed it to `10.0.0.0/24` |
