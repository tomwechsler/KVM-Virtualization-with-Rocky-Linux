Before we can dive in and start working with oVirt, we will need to configure a Rocky 9 environment.  In this lesson, we will walk through setting up a 'Minimal' installation of Rocky 9.

#### Reference Links

**Download Rocky 9:**

[Rocky Mirrors List](https://mirror.puzzle.ch/rockylinux/9.3/isos/x86_64/)

**Create USB Key:**

[How to create a bootable USB drive for Linux?](https://www.poweriso.com/tutorials/how-to-make-linux-bootable-usb-drive.htm)  

### Configuring the Rocky 9 Lesson Environment

#### For this section, I will use a bare-metal environment on one HP Elite workstation

**Specifications:**

- Intel CPU (4 core)
- 28GB RAM
- 256GB SSD
- 2x 500GB SSD
- Two-gigabit network interfaces
- Rocky 9 (Minimal Installation)

This is a 10-year-old machine and works perfectly for this lesson.

#### First, you will need to download Rocky 9 and create installation media

1. Download the Rocky 9 Minimal ISO.  We're going to work relative to this, so it's important you start with the Minimal installer.

2. Next, you will need to decide what kind of installation media you are going to use and create it.  I usually use a USB installation as it is easier and more environmentally-friendly.  If you need to create a CD or DVD from the ISO file, you can do that as well.

#### Start the installation

1. Either plug in your USB installer key or insert your CD/DVD into the drive and boot your computer.

2. On the way up, we want to enter the BIOS to confirm that hardware-assisted virtualization is enabled.  I'm not going to go into detail on how you do that as there are numerous ways, depending on your particular computer.  Go get a copy of the hardware manual and read up.  It will tell you what you need to enable.

3. Once you exit the BIOS (save your changes!) you will want to select a boot menu —if your computer has that option— and boot from your installation media.

4. When the Rocky 9 installer starts, you can skip the media check if you wish.

#### Installing Rocky 9

1. With this Rocky 9 installer, the only package choice is *Minimum*.  The installer is similar to the installer we used to install Rocky 9, so setup will be the same.

2. Set a `root` password and create the `tom` user during installation.

#### Post-installation tasks

1. After the system has rebooted, log in as 'tom'.  We need to patch the system:
    ```
    sudo dnf -y update
    ```
2. After patching is complete, reboot the system:
    ```
    sudo systemctl reboot
    ```

#### Configure Storage and NFS Server

After the system has rebooted, log in as `tom`.  We're going to configure four storage locations, one for the virtual machine images, one for the hosted engine, one for the export domain, and one for the ISO store.  I'm going to create new logical volumes and file systems, but you can just create directories to use if you'd like.

#### Configure the storage:

1. Let's create the mount points:
    ```
    sudo mkdir /media/ISO
    ```
    ```
    sudo mkdir /media/VM_Store
    ```
    ```
    sudo mkdir /media/HE_Store
    ```
    ```
    sudo mkdir /media/Export_Store
    ```
2. Next, I'm going to create some new logical volumes using an additional 3TB disk that is in the system.  This disk is `/dev/sdc`.

3. Clean this disk:
    ```
    sudo wipefs -a /dev/sdc
    ```
4. Add this disk to LVM:
    ```
    sudo pvcreate -ff /dev/sdc
    ```
5. Let's check our work:
    ```
    sudo pvs
    ```
6. Next, let's add `/dev/sdc` to a new volume group, 'ovirt':
    ```
    sudo vgcreate ovirt /dev/sdc
    ```
7. Let's check our work:
    ```
    sudo vgs
    ```
8. Create our logical volumes:
    ```
    sudo lvcreate -L 200g -n ISO ovirt
    ```
    ```
    sudo lvcreate -L 200G -n VM_Store ovirt
    ```
    ```
    sudo lvcreate -L 200g -n HE_Store ovirt
    ```
    ```
    sudo lvcreate -L 200g -n Export_Store ovirt
    ```
9. Let's check our work:
    ```
    sudo lvs
    ```
10. Next, let's format the new logical volumes:
    ```
    sudo mkfs.xfs /dev/mapper/ovirt-ISO
    ```
    ```
    sudo mkfs.xfs /dev/mapper/ovirt-VM_Store
    ```
    ```
    sudo mkfs.xfs /dev/mapper/ovirt-HE_Store
    ```
    ```
    sudo mkfs.xfs /dev/mapper/ovirt-Export_Store
    ```
11. Now we will need to edit `/etc/fstab` in order to add the mounts for our new logical volumes.
    ```
    sudo vi /etc/fstab
    ```
12. Add the entries for the new mount, then save and exit.

13. Now, let's mount our new filesystems:
    ```
    sudo mount -a
    ```
14. Let's check our work:
    ```
    df -h | grep media
    ```

#### Configure the NFS exports:

1. First, we will need to install the packages required for the NFS server:
    ```
    sudo dnf -y install nfs-utils
    ```
2. Enable and start the NFS services:
    ```
    sudo systemctl enable rpcbind --now
    ```
    ```
    sudo systemctl enable nfs-server --now
    ```
3. We will need to configure the firewall to allow NFS:
    ```
    sudo firewall-cmd --permanent --zone=public --add-service=nfs
    ```
    ```
    sudo firewall-cmd --permanent --zone=public --add-service=mountd
    ```
    ```
    sudo firewall-cmd --permanent --zone=public --add-service=rpc-bind
    ```
    ```
    sudo firewall-cmd --reload
    ```
4. Let's check our work:
    ```
    sudo firewall-cmd --list-all
    ```
5. Next, we need to create `/etc/exports` and add our new filesystems:
    ```
    sudo vi /etc/exports
    ```
6. Add the following lines:
    ```
    /media/ISO    192.168.10.0/24(rw,sync,no_root_squash,no_all_squash)
    /media/VM_Store    192.168.10.0/24(rw,sync,no_root_squash,no_all_squash)
    /media/HE_Store    192.168.10.0/24(rw,sync,no_root_squash,no_all_squash)
    /media/Export_Store    192.168.10.0/24(rw,sync,no_root_squash,no_all_squash)
    ```
7. Save, and exit.

8. Restart the NFS server:
    ```
    sudo systemctl restart nfs-server
    ```
9. Let's check our work:
    ```
    showmount -e
    ```
10. We should see the four exports we just configured.  We're good to proceed!
