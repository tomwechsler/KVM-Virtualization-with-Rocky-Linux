We've looked at two ways we can use `libvirt` to administer virtual machines via QEMU/KVM, Virtual Machine Manager (`virt-manager`), and Cockpit.  While these were easy to use, sometimes we need the granularity and power that command-line tools provide.  Fortunately for us, `libvirt` has a set of command-line tools to round out our toolkit.  In this lesson, we will take a look at using `virsh` to manage virtual devices.

#### Reference Links

[libvirt: virsh](https://libvirt.org/manpages/virsh.html#synopsis)

### Using `virsh` to Manage Virtual Devices

As part of the preparation of our new CentOS 7 virtual machine template for the DBA team, we need to modify the virtual machine's configuration. 

#### Connecting to the 'virsh' serial console

We used a serial console when we installed Rocky 9.  Let's take a look at how to connect and disconnect from the serial console, in case we need a console connection to our virtual machine.

##### Connect to the console:

1. If the virtual machine isn't started, start the virtual machine:
    ```
    sudo virsh list --all
    ```
    ```
    sudo virsh start rocky-9-cli-vm
    ```
2. Let's connect to the `virsh` console:
    ```
    sudo virsh console rocky-9-cli-vm
    ```
3. Log in as `tom`.

##### Disconnect from the console:

Use **CTRL** + **]** to exit the `virsh` console.

#### Attaching an ISO file to a virtual machine

Sometimes we need to attach a disk image file to the virtual `cdrom` drive.  We can do that from the command line using `virsh`.  Let's attach our Rocky 9 Boot media in `/tmp` to our 'rocky-9-cockpit-vm' domain.

1. Start the `rocky-9-cockpit-vm` domain, if it's not already running:
    ```
    sudo virsh list --all
    ```
    ```
    sudo virsh start rocky-9-cockpit-vm
    ```
2. Confirm the virtual media drive device name:
    ```
    sudo virsh domblklist rocky-9-cockpit-vm
    ```
3. Attach the ISO file:
    ```
    sudo virsh change-media rocky-9-cockpit-vm sda /tmp/Rocky-9-latest-x86_64-boot.iso --insert --live
    ```
    This will attach the Rocky 9 Boot ISO.  If your ISO has a different name, substitute it.

4. Log into the `rocky-9-cockpit-vm` virtual machine via SSH as 'tom'.  We will need to get the IP address of the VM:
    ```
    sudo virsh net-dhcp-leases default
    ```
5. Let's confirm that we can mount the ISO.  First, check for the virtual disk drive:
    ```
    ls -la /dev/sr0
    ```
6. Confirm that the device exists.  Let's check that the `/mnt` directory is empty so we can use it as a mount point:
    ```
    ls -la /mnt
    ```
7. If it is empty, go ahead and mount the ISO to `/mnt`:
    ```
    sudo mount /dev/sr0 /mnt
    ```
8. Confirm that the ISO file is mounted:
    ```
    df -h | grep mnt
    ```
    ```
    ls -al /mnt
    ```
    We're not going to install any software at this point; this was just practice.

9. Unmount the ISO file:
    ```
    sudo umount /mnt
    ```
10. Log out of the virtual machine.

11. Detach the ISO file from the virtual media drive:
    ```
    sudo virsh change-media rocky-9-cockpit-vm sda --eject
    ```
12. Confirm it was ejected:
    ```
    sudo virsh domblklist rocky-9-cockpit-vm
    ```
13. Shut down the `rocky-9-cockpit-vm` domain:
    ```
    sudo virsh shutdown rocky-9-cockpit-vm
    ```
    ```
    sudo virsh list --all
    ```

#### Edit guest domain XML file

A virtual machine's configuration is stored in an XML file in `/etc/libvirt/qemu/`.  We can edit it directly using the `virsh edit` command.

We gave our virtual machine plenty of memory for installation, but it's going to require more memory and CPUs to serve as a database server.

1. Before we make any edits, we will need to shut down the guest:
    ```
    sudo virsh shutdown rocky-9-cli-vm
    ```
2. We can adjust these by editing the XML configuration file for our virtual machine:
    ```
    sudo virsh edit rocky-9-cli-vm
    ```

##### Change the virtual memory size

Adjust the virtual memory to 8GB (or whatever your system can spare)

##### Adjust the number of vCPUs

Adjust the number of vCPUs to 4 (or whatever your system can spare)

##### Apply the changes

Save the new configuration.

1. Start the domain:
    ```
    sudo virsh start rocky-9-cli-vm
    ```
2. Connect to the `virsh` console:
    ```
    sudo virsh console rocky-9-cli-vm
    ```
3. Log in as 'tom'.

4. Confirm the memory and CPU count via the *operating system*:
    ```
    grep -i processor /proc/cpuinfo | wc -l
    ```
    ```
    grep MemTotal /proc/meminfo
    ```
5. Log out and disconnect from the console.  Use **CTRL**+**]** to exit the `virsh` console.

6. Confirm the changes via `virsh`:
    ```
    sudo virsh dominfo rocky-9-cli-vm
    ```

#### Congratulations, you're one step closer to handing over the virtual machine to the DBA team!