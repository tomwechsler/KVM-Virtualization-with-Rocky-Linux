#### Reference Links

**Download Rocky 9:**

[Rocky Mirrors List](https://mirror.puzzle.ch/rockylinux/9.3/isos/x86_64/)

**Create USB Key:**

[How to create a bootable USB drive for Linux?](https://www.poweriso.com/tutorials/how-to-make-linux-bootable-usb-drive.htm)  

#### How to Create the Rocky 9 Workstation Lesson Environment From Scratch

**We will need a Rocky 9 Boot ISO for installation**

Download the latest **Rocky 9 Boot ISO** (...-boot.iso) from the mirror list above.

**We will need to create a *USB or optical* installation media if we are installing on physical hardware**

1. Create either a Rocky USB installation key or a CD/DVD installation disc.  Boot your computer from your installation media.

2. On the way up, check your BIOS to make sure that virtualization is enabled:

- Intel VT-x
- AMD-V

*For more information configuring these on your computer, reference the appropriate documentation for your particular system.*

**Install Rocky 9.  Choose the 'Workstation' option.  Set the 'root' password and create the 'kvm_user' account.  Reboot.**

1. Log in as 'kvm_user'.

2. Before we install anything, we'll confirm we have virtualization support:
```
lscpu | grep Virtualization
```

***Optional* - Install the `xrdp` service to provide RDP access, if you'd like.**

3. Set up the `epel-release` repository:
```
sudo dnf -y install epel-release
```
4. Install the `xrdp` service
```
sudo dnf -y install xrdp
```
5. Configure the `xrdp` service to start at boot, and start it:
```
sudo systemctl enable xrdp --now
```
```
sudo systemctl status xrdp
```
*Configure the firewall to allow traffic for the `xrdp` service*

6. Let's check the firewall status:
```
sudo firewall-cmd --list-all
```
7. Add a rule to allow traffic on port 3389:
```
sudo firewall-cmd --zone=public --add-port=3389/tcp --permanent
```
8. Activate the new firewall configuration:
```
sudo firewall-cmd --reload
```
9. Once more, check the firewall status:
```
sudo firewall-cmd --list-all
```
10. Now you can test a connection with your RDP client of choice.