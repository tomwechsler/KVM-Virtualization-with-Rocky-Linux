# Description

Cockpit is a web-based management utility that gives users a web interface to manage their server.  Using the 'cockpit-machines' plugin, you can use Cockpit to manage virtual machines as well.  In this lesson, we will walk through installing and configuring Cockpit on Rocky 9 and will take a quick tour of Cockpit, focusing on the 'Virtual Machines' pane.

#### Reference Links

**Cockpit:**
- [Running Cockpit - Cockpit Project](https://cockpit-project.org/running.html)
- [Multiple Machines - Cockpit Project](https://cockpit-project.org/guide/latest/feature-machines.html)
- [Applications - Cockpit Project](https://cockpit-project.org/applications.html)

**Download Rocky 9:**
[Rocky Mirrors List](https://mirror.puzzle.ch/rockylinux/9.3/isos/x86_64/)

**Create USB Key:**
[How to create a bootable USB drive for Linux?](https://www.poweriso.com/tutorials/how-to-make-linux-bootable-usb-drive.htm)  

### Installing and Configuring Cockpit on Rocky 9

We're going with a leaner, more "server-like" version of Rocky 9 for the Cockpit and CLI sections.  Start with a fresh Rocky 9 **'minimal'** installation on your computer, then proceed with the lesson.  If you need a refresher, see the "How to Create the Rocky 9 Workstation Lesson Environment From Scratch" lesson.

#### Install 'cockpit' and the 'cockpit-machines' plugin

By default, **Cockpit** is installed on many Redhat distributions. You can check to see if it is installed by running:
```
sudo dnf list installed | egrep -i "cockpit|virt"
```
This will also check for virtualization packages.

1. We will need to install `libvirt`, the `cockpit`, and `cockpit-machines` packages and any dependencies:
```
sudo dnf -y install libvirt cockpit cockpit-machines
```
2. Now would also be a good time to check for updates:
```
sudo dnf -y update
```
3. Confirm that the firewall is configured to allow **Cockpit** traffic to pass:
```
sudo firewall-cmd --list-all
```
4. You should see `cockpit` listed in `services`.  *If it is not*, execute the following to allow communication:
    ```
    sudo firewall-cmd --zone=public --add-service=cockpit --permanent
    ```
    ```
    sudo firewall-cmd --reload
    ```

#### Configuring Permissions for Non-root Users

We need to do some work to make Cockpit work for non-root users:

1. Let's add `tom` to the `libvirt` group:
    ```
    sudo usermod -a -G libvirt `id -un`
    ```
2. Add `tom` to the `libvirtdbus` group:
    ```
    sudo usermod -a -G libvirtdbus `id -un`
    ```
3. Add the `libvirtdbus` user to the `libvirt` group:
    ```
    sudo usermod -a -G libvirt libvirtdbus
    ```
4. Add the `qemu` user to the `libvirt` group:
    ```
    sudo usermod -a -G libvirt qemu
    ```

#### Start Everything!

Now, let's start the `cockpit.socket` and `libvirtd` services:

1. Enable and start **Cockpit**:
    ```
    sudo systemctl enable cockpit.socket --now
    ```
2. Check the status of `cockpit.socket`:
    ```
    sudo systemctl status cockpit.socket
    ```
3. Enable and start `libvirtd`:
    ```
    sudo systemctl enable libvirtd --now
    ```
4. Check the status of `libvirtd`:
    ```
    sudo systemctl status libvirtd
    ```

#### Virtual Disk Storage Location

By default, virtual disks are stored in `/var/lib/libvirt/images`.  This is in the `root` (`/`) filesystem.  We don't want to fill this filesystem up, so we're going to create a directory on the `/home` filesystem to store virtual disks.

Create a dedicated directory for our virtual disks:

1. Create the directory:
    ```
    sudo mkdir /home/vms
    ```
2. Set the proper ownership:
    ```
    sudo chown -R qemu:libvirt /home/vms
    ```
3. Set the proper permissions:
    ```
    sudo chmod 0770 /home/vms
    ```
4. Confirm we have what we want:
    ```
    sudo ls -la /home
    ```
5. Launch Firefox and connect to Cockpit from another computer on your network ***on port 9090***.  Use Firefox if you can, as Chrome will not accept the SSL certificate.

#### Tour the Cockpit web interface

1. Log in to the Cockpit web interface using the `tom` account and take a look around.  
3. On the left-hand side, click on the **Virtual Machines** pane.
