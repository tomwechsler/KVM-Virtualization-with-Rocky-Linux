The Virtual Machine Manager (virt-manager) is a desktop virtual machine manager that gives users a graphical user interface to manage virtual machines.  In this lesson, we will walk through creating a new virtual machine using 'virt-manager'.

#### Reference Links

[Rocky Mirrors List](https://rockylinux.org/de-DE/download)

### Creating a Virtual Machine Using 'virt-manager'

1. The first thing we need to do is download the Rocky 9 Boot ISO for installation:
    1. Open Firefox, and paste the link for the *Rocky Mirrors List* found above into the URL location and go there.  
    2. Choose a mirror, then Download the *Boot ISO* (should be about 900M).  
    3. Save the file.  This will be in the `tom` *Downloads* directory (`/home/tom/Downloads`).

2. Next, let's use the ISO to create a new virtual machine:

    1. Open Virtual Machine Manager.  Click on the *Create a new virtual machine* icon in the upper left-hand corner, under the *File* menu.
    2. Virtual Machine Manager will open the "New VM" wizard.  Select **Local install media** and click **Forward**.
    3. We need to select the Boot ISO file we downloaded.  Click on the **Browse** button.  This will open the *Choose Storage Volume* window.
    4. Click on the **Browse Local** button.  This will open the *Locate ISO media* window.
    5. Select **Downloads** from the left-hand column, where the directories are located.  You should see the Rocky 9 Boot ISO.  Make sure it is selected.  Click on the **Open** button in the top right-hand corner of the window.
    6. Uncheck *Automatically detect from the installation media/source.*  Type "Generic" in the search box.  Select **Generic default**.  Click on the **Forward** button.
    7. Set the memory setting to what you can spare - 1G or more should be good.  Set CPUs to a minimum of *1*.  Click on the **Forward** button.
    8. Next, we'll create a disk image.  Set this to 10Gib.  Click on the **Forward** button.
    9. Set the name to `rocky-9-vm`.  Check the *Customize configuration before install* box.  Click the arrow next to *Network selection*.  Use the *Virtual network 'default'* option.  Click the **Finish** button.
    10. You will be presented with the overall hardware configuration.  We will walk through all the options.  We'll keep the virtual machine as configured, with the following exceptions:
        1. Set the disk controller for the virtual disk "IDE Disk 1" to "VirtIO"
            - Advanced options --> Disk bus:
        3. Set the virtual network adapter to 'virtio':
            - Network source
    11. Start the installation.  Click the **Begin Installation** button in the upper left-hand corner of the window.
        1. Set the virtual console to fullscreen.
        2. Skip installation media testing
 
 It might take a while to boot.  Be patient.

3. Walk through the installation.  Make sure you select the following:

    - Time & Date
      - Time zone (same as the parent server)
    - Network & Host Name
      - Turn on network
      - Set the hostname to `rocky-9-vm`
    - Software Selection
      - Select **Minimal Install**
    - Installation Destination
      - Use defaults.  Click on the 10GiB disk you created.

4. Once these have been selected, the *Begin Installation* button in the lower right-hand corner should be blue.  Click it.

5. During the installation, configure the following:
    - Set the `root` password to the same as the parent VM, or whatever you want
    - Create the `tom` user:
      - *Full name:* Tom Wechsler
      - *User name:* tom
      - Check *Make this user administrator*
      - Check *Require a password to use this account*
      - Set a password for the account:
        - You can either use the same password as the `tom` account on the parent server or set your own

6. Once the installation is complete, reboot the VM.  Once the VM is back up, log in as the `tom` user with the password you created and power down the VM:
```
sudo systemctl poweroff
```
7. Close the VM console window.
