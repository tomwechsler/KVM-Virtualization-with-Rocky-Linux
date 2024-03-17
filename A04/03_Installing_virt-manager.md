The Virtual Machine Manager (virt-manager) is a desktop virtual machine manager that gives users a graphical user interface to manage virtual machines.  In this lesson, we will walk through installing and configuring `virt-manager` on Rocky 9.

### Installing and Configuring 'virt-manager' on Rocky 9

#### Install 'virt-manager' and associated components:

1. First, let's become `root`:
```
sudo su -
```

2. Show installation information:
```
dnf groupinfo Virtualization\ Client
```
3. Observe the packages in the group: 

*Mandatory / Default / Optional*

4. Install the 'Virtualization Client' group:
```
dnf -y group install --with-optional Virtualization\ Client
```
5. Configure the `tom` user and `libvirtd` to allow non-root access:

6. Let's edit `/etc/libvirt/libvirtd.conf`:
```
vim /etc/libvirt/libvirtd.conf
```
7. Assign the domain socket group ownership:

`unix_sock_group = "libvirt"`

8. Let's give the `libvirt` group R/W access to the socket:

`unix_sock_rw_perms = "0770"`

> Save and exit the file

9. Now, let's add `tom` to the `libvirt` group:
```
usermod -a -G libvirt tom
```
10. We need to log out and log back in now to pick up the changes.

11. Let's edit `/etc/libvirt/qemu.conf`

12. vim /etc/libvirt/qemu.conf

13. Replace the following:

`#user = "root"` replace root with your username and remove the comment

`#group = "root"` replace root with the groupname libvirt and remove the comment

> Save and exit the file

#### Validate the installation

1. Confirm `libvirtd` is enabled and running:
```
systemctl status libvirtd && systemctl enable --now libvirtd
```

2. Confirm KVM kernel modules are loaded:
```
lsmod | grep -i kvm
```

#### Confirm KVM support

1. Confirm that the `/dev/kvm` device exists:
```
ls -la /dev/kvm
```
2. Check for Intel VT-x / AMD-V support in the processor:
```
egrep "svm|vmx" /proc/cpuinfo
```
Let's take a quick tour!