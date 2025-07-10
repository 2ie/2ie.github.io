# Escami Homelab

Welcome! This repository is where I will document my homelab, what all I have done to it, and the problems—and solutions—that I have encountered. (**This same info is also on [escami.com](https://escami.com)**)

---

## Intro:

What are the **specs of the server?**

> The server is a Beelink SER5 mini-PC powered by a Ryzen 5 5560U, a 6-core/12-thread AMD CPU. It also has 16GB of DDR4 memory, 500GB NVME SDD, and has a single RJ45 port. You can find this same mini-PC by googling B0B2943QSJ.

What is the **server running?**

> The server is running a Type 1 or "bare metal" Hypervisor called Proxmox Virtual Environment (VE), it is currently running Proxmox VE version 8.4.1. Proxmox is open-source, quite intuitive, and remotely accessible from within your home network.

---

## Projects:

#### **Self-Hosted Network Attached Storage (NAS):**

The _how?_

> 1. Create an LXC Container running a Debian 12, and assign it a <u>static</u> IP address.
> 2. Back in that Debian container, install Cockpit, which allows us to more easily access the shares and users through a user-interface. `apt install -t cockpit --no-install-recommends`
> 3. Before we can access the web UI, we need to allow the root user to access cockpit. Remove `root` from the file. `nano /etc/cockpit/disallowed-users`
> 4. Verify that we can successfully connect to the web user-interface, whatever static IP address you used followed by port 9090. Login using your root username and password.
> 5. Install these three Cockpit plugins to give us more user management options and overall ease-of-use. See the following repos for more information: [cockpit-file-sharing](https://github.com/45Drives/cockpit-file-sharing "GitHub repo for cockpit-file-sharing plugin"), [cockpit-navigator](https://github.com/45Drives/cockpit-navigator "GitHub repo for cockpit-navigator plugin"), and [cockpit-identities](https://github.com/45Drives/cockpit-identities "GitHub repo for cockpit-identities plugin").
> 6. In Proxmox, mount your desired disk for use in the network storage.
> 7. In Cockpit, create desired users and groups under the new "Identities" tab.
> 8. In Cockpit, we also need to create a network share under "File Sharing", this share will be the directory for your NAS. Assign appropriate restrictions for users and groups.
> 9. Lastly, verify that you can access your NAS remotely through File Explorer.

Yea but _why?_

> I record a lot of videogame content with my friends, and on longer playthroughs, those recording file sizes add up. I do editing on both my home desktop PC, and my laptop. This meant it was a pain trying to move these video files back-and-fourth, from cloud storage services like Google Drive. Self-hosting a NAS made sense for this purpose, as I wanted to be able to access this footage from any device anywhere in the home.

---

## Home*<u>labs</u>*:

#### **Windows Server Active Directory Lab:**

The _how?_

> 1. Provision a Windows Server 2025 (wserver) virtual machine, make sure to assign a static IP address.
> 2. In wserver, initialize the Domain Controller services, in "Server Roles", be sure to select "Active Directory Domain Services" and "DNS Server".
> 3. Promote the server to a Domain Controller, create a new forest, assign a root domain name.
> 4. Create Users, Groups, and Organizational Units in Active Directory Users and Computers.
> 5. Create a Windows 11 Pro (wpro) virtual machine, in the system, update the preferred DNS to wserver's IP.
> 6. In wpro, under system properties, change wpro to be a member of the domain that was previously created on wserver. The VM will restart, and upon restart, you can now login to the domain as one of those test users.
> 7. Make neccessary edits, test account security policies.

Yea but _why?_

> I did this lab to better familiarize myself with the basics of Windows Server 2025 and Active Directory. This lab was also my first time creating a Domain Controller, Organizational Units, Group Policy Objects, and connecting other external VMs to this domain. There is a lot you can do, from editing permissions on users, groups, computers, to implementing audit policies, these systems offer a lot of flexibility, and you can make a system as secure as needed.

---

## General Homelab Woes:

- Dependency problems can feel like a videogame fetch quest. Where trying to fix one issue leads you down a rabbit-hole troubleshooting other problems before you can progress.

- Windows 11 requiring an internet connection and online user account for virtual machines. Temporarily disabling the network device and rebooting setup bypassed their check. Linux installs are 10x easier.

- The desire for a bigger and better homelab server. This server is adequate for the time-being, but, as I continue to provision more VMs, I can see the amount of cores or memory bottlenecking the server.

---

## Future:

- [ ] Pterodactyl Videogame Server Manager
- [ ] PFsense Firewall / VPN
