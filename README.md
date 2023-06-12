# ThinClient_For_Proxmox
Proxmox is a powerful server virtualization platform. This tutorial explains how to set up Proxmox on a server and access its virtual machines (VMs) from other nodes. It covers installation, configuration, and remote access, enabling seamless management and utilization of VMs across your network.
# Prerequisites
1) PAM authenticated user with appropriate VM rights
2) Debian image (ISO) Flashed to a USB
# Steps
Installing Debian Image on node
1) Start the installation with the Debian Netinst image, choosing the "Install" option instead of "Graphical Install."
2) During the installation process, do not set a root password. This will ensure that Debian installs sudo, allowing for administrative privileges.
3) reate a new user, which will serve as the administrative user with sudo permissions. Select a strong username that is not intended for use as a virtual session user.
4) hoose the "Guided" option, and select "Use Entire Disk" as the partition type.
5) pt for the "All files in one partition" setup, which will create a single partition for the installation.
Eventually, it will ask you if you would like to install a desktop environment.
1) Deselect GNOME
2) Select LXDE
3) Select SSH server

After reboot

open the terminal and execute

<code>sudo nano /etc/lightdm/lightdm.conf</code>

Navigate to the section containing "#greeter-hide-users=false" and remove the "#" symbol.

This adjustment ensures that the user list is visible instead of being hidden.

Clone the script in your user's home directory and make it executable

<code> git clone https://github.com/KBapna/ThinClient_For_Proxmox.git && cd ThinClient_For_Proxmox && cp thinclient /usr/local/bin/thinclient && chmod +x /usr/local/bin/thinclient </code>

Edit the script according to the comments in script

<code>nano /usr/local/bin/thinclient</code>

Execute the following code for dependecies

<code>sudo apt install virt-viewer curl</code>

As we will use Debian's own UI to login we can customize the script accordingly for each user and keep it in their home directory.
But first we will need to create those users. I will create a test users to access VM 101 for demo.

<code>sudo adduser (username)</code>

We can also setup passwordless login by editing the following file <code>/etc/pam.d/lightdm</code> and add the following line right after #%PAM-1.0 at the top of the file

<code>auth    sufficient  pam_succeed_if.so user ingroup nopasswdlogin</code>

and create the group <code>sudo groupadd -r nopasswdlogin</code>

Now, for each no-password virtual session user, add them to the group nopasswdlogin

<code>sudo gpasswd -a (username) nopasswdlogin</code>

Setting up LXDE for AUTORUN

<code>sudo mv /etc/xdg/lxsession/LXDE/autostart /etc/xdg/lxsession/LXDE/autostart.bak
sudo touch /etc/xdg/lxsession/LXDE/autostart
mkdir -p ~/.config/lxsession/LXDE
cp /etc/xdg/lxsession/LXDE/autostart.bak ~/.config/lxsession/LXDE/autostart</code>

Login to each user by SSH and execute the following

<code>mkdir -p ~/.config/lxsession/LXDE
nano ~/.config/lxsession/LXDE/autostart</code>

Contents of the file:

@/usr/bin/bash /usr/local/bin/thinclient 101

Replace 100 with the VM ID you want this user to be launch.
