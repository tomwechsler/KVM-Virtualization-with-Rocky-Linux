The Virtual Machine Manager (virt-manager) is a desktop virtual machine manager that gives users a graphical user interface to manage virtual machines.  In this lesson, we walk through two common storage operations: adding a virtual disk and attaching and mounting an ISO image.

### Managing Storage Using `virt-manager`

#### Adding a virtual disk to a virtual machine

Our web developer has requested a dedicated directory for his content.  We don't want to rob from the operating system, so we're going to add a new virtual disk.

1. Take a look at the LVM configuration:
    ```
    sudo pvs
    ```
    ```
    sudo lvs
    ```
    ```
    sudo vgs
    ```
    ```
    ls -al /dev/vd*
    ```
2. Add a new virtual disk to the VM.  Check for the new virtual disk:
    ```
    ls -al /dev/vd*
    ```
3. Take a look at the partition table of the new virtual disk:
    ```
    sudo fdisk -l /dev/vdb
    ```
4. Create a new physical volume:
    ```
    sudo pvcreate /dev/vdb
    ```
5. Add the new virtual disk to a new volume group, `web-vg`:
    ```
    sudo vgcreate web-vg /dev/vdb
    ```
6. Create a new logical volume, `web-content`:
    ```
    sudo lvcreate -L 5G -n web-content web-vg
    ```
7. Take another look at the LVM configuration:
    ```
    sudo pvs
    ```
    ```
    sudo lvs
    ```
    ```
    sudo vgs
    ```
8. Format the new logical volume:
    ```
    sudo mkfs.xfs /dev/mapper/web--vg-web--content
    ```
9. Create the new mountpoint, `/web`:
    ```
    sudo mkdir /web
    ```
10. Edit `/etc/fstab`, add a mount for the `web-content` LV at `/web`:
    ```
    sudo vi /etc/fstab
    ```
11. Mount the new LV:
    ```
    sudo mount -a
    ```
12. Confirm that the `/web` filesystem is mounted:
    ```
    df -h
    ```

#### Attach an ISO disk image to a virtual machine

1. Attach the ISO to the virtual machine.  Check for the CDROM device:
    ```
    ls -la /dev/sr0
    ```
2. Confirm that the `/mnt` directory is empty:
    ```
    ls -la /mnt
    ```
3. Mount the ISO on `/mnt`:
    ```
    sudo mount /dev/sr0 /mnt
    ```
4. Show the contents of the ISO:
    ```
    ls -al /mnt
    ```
5. Unmount the ISO from `/mnt`:
    ```
    sudo umount /mnt
    ```
6. Remove the ISO from the VM via `virt-manager`.

Now, we have a happy Web Developer!