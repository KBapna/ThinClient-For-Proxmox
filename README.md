# ThinClient_For_Proxmox
Proxmox is a powerful server virtualization platform. This tutorial explains how to set up Proxmox on a server and access its virtual machines (VMs) from other nodes. It covers installation, configuration, and remote access, enabling seamless management and utilization of VMs across your network.
# Prerequisites
Debian image (ISO) Flashed to a USB
# Steps
Installing Debian Image on node
1) Start the installation with the Debian Netinst image, choosing the "Install" option instead of "Graphical Install."
2) During the installation process, do not set a root password. This will ensure that Debian installs sudo, allowing for administrative privileges.
3) reate a new user, which will serve as the administrative user with sudo permissions. Select a strong username that is not intended for use as a virtual session user.
4) hoose the "Guided" option, and select "Use Entire Disk" as the partition type.
5) pt for the "All files in one partition" setup, which will create a single partition for the installation.
