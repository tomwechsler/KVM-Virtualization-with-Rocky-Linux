# Description

Cockpit is a web-based management utility that gives users a web interface to manage their servers.  Using the `cockpit-machines` plugin, you can use Cockpit to manage virtual machines as well.  In this lesson, we will walk through creating a new virtual machine with Cockpit, including installing a 'minimal' Rocky 9 instance.

#### Reference Links

**Download Rocky 9:**
[Rocky Mirrors List](https://mirror.puzzle.ch/rockylinux/9.3/isos/x86_64/)

### Creating a Virtual Machine Using Cockpit

#### Get the Rocky 9 Boot ISO

We will need a Rocky 9 Boot ISO for installation

1. Download the latest **Rocky 9 Boot ISO** (...-boot.iso) from the mirror list above.  
```
curl --output rock9.iso https://download.rockylinux.org/pub/rocky/9/isos/x86_64/Rocky-9.3-x86_64-boot.iso
```
2. Save or copy it to `/tmp` on your system.

#### Log in to Cockpit

Log in to the Cockpit web interface at `http://<YOUR_HOST_IP>:9090`

#### Add the `/home/Virtual_Machines` directory to the Storage Pools

1. In the upper-right hand corner of the 'Virtual Machines' pane, you will see the link to the **Storage Pools**.  Click on this link.

2. In the upper right-hand corner of the 'Storage Pools' pane, you will see a button labeled *Create Storage Pool*.  Click it.

**Settings:**

- *Connection:* QEMU/KVM System connection
- *Name:* `Virtual_Machines`
- *Type:* Filesystem Directory
- *Target Path:* `/home/Virtual_Machines`
- *Startup:* CHECKED

3. Click **Create**.  You should see your new `Virtual_Machines` storage pool.  Click on the **Virtual Machines** link in the upper left-hand corner of the pane to go back to the *Virtual Machines* pane.

#### Create a New Virtual Machine

1. In the upper right-hand corner of the *Virtual Machines* pane, click on the **Create VM** button.

2. We're going to use the following values for the new VM:

    - Connection: QEMU/KVM System connection
    - Name: rocky-9-cockpit-vm
    - Installation Source Type: Local Install Media
    - Installation Source: /tmp/rocky...  (exact file name will depend on your download)
    - Storage: No Storage (we will add this later as we are not using the default location)
    - Memory: Set this to an appropriate value for your system
    - OS Vendor: Rocky
    - Operating System: Rocky 9
    - Immediately Start VM: UNCHECKED

3. Click on the **Create** button.  The **Create New Virtual Machine** window will disappear, and you will be back in the *Virtual Machines* pane.

#### Add a Virtual Disk

1. Click on the entry for the new `rocky-9-cockpit-vm`.  Click on the **Disks** tab.  Click on the **Add Disk** button on the right.

2. Set the Add Disk` Settings:

    - *Source:* Create New
    - *Pool:* `Virtual_Machines`
    - *Name:* `rocky-9-cockpit-vm`
    - *Size:* Set this to an appropriate value for your system
    - *Format:* `qcow2`
    - *Show Performance Options:* LEAVE DEFAULT(S)

3. Click on **Add**.

#### Launch the Rocky 9 Installation

1. Click on the **Consoles** tab, then click on the **Install** button on the right-hand side.  This will launch the installation.  Click in the console window to enable input.

2. We're going to perform a 'Minimal' Rocky 9 installation.  Once the installation is complete, click on the **Reboot** button in the installer.  This will shut down the VM.

#### Start the New VM

1. Now that installation is complete, and the VM is shut down, we can click on **Run** on the right-hand side and start the new VM.

2. Once the VM is up and running, click on the console window and log in as `tom`.  Let's patch the system if required:
    ```
    sudo dnf -y update
    ```

3. Shut down the VM:
    ```
    sudo systemctl poweroff
    ```

Congratulations on creating your first VM with Cockpit!