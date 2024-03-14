Cockpit is a web-based management utility that gives users a web interface to manage their servers.  Using the `cockpit-machines` plugin, you can use Cockpit to manage virtual machines as well.  In this lesson, we will walk through managing our new virtual machine.

### Managing Virtual Machines Using Cockpit

Now that we have our new VM, we'd like to make some changes.  We'd like to add CPU cores, memory, and a second disk.

If you're not already logged into **Cockpit**, do so now, and go to the 'Virtual Machines' pane.

#### Add CPU Cores / Memory to the VM

Our initial CPU and memory configuration was just fine for installing Rocky, but we'd like to give this virtual machine more resources.

1. Click on `rocky-9-cockpit-vm`.  In the *Overview* tab, click on the value for *Memory* and adjust the *Current Allocation* and *Maximum Allocation* (up if you can spare it, down if not).  Click the **Save** button.

2. Next, click on the value for `vCPUs`.  Adjust the *vCPU Count* and *vCPU Maximum* values (up if you can spare it, down if not).  Click the **Apply** button.

3. Click on the **Consoles** tab and start the VM.  Once the server is up, log in as 'cloud_user'.

4. Verify that the memory size is what we adjusted it to:
    ```
    grep MemTotal /proc/meminfo
    ```
5. Verify that we now have the proper number of vCPUs:
    ```
    grep -i processor /proc/cpuinfo | wc -l
    ```
Looking great so far!

#### Add a Disk to the VM

Adding a disk is pretty straightforward.  Let's take a look.

Before we add our new disk, let's take a look at what we have on the system already:

    - List of disks, using `fdisk`:
        ```
        sudo fdisk -l
        ```
    - List of virtual disk devices:
        ```
        ls -la /dev/vd*
        ```
1. In the *Disks* tab, click on **Add Disk**.

2. Set the *Values*:

    - *Source:* Create New
    - *Pool:* Virtual_Machines
    - *Name:* rocky-9-cockpit-vm-disk2
    - *Size:* BASED ON RESOURCES
    - *Format:* qcow2
    - *Performance Options:* LEAVE AS DEFAULT

3. Click on **Add**.  Click on the **Console** tab.

4. Let's take a look at what we have *now*:

   - List of disks, using `fdisk`:
        ```
        sudo fdisk -l
        ```
   - List of virtual disk devices:
        ```
        ls -la /dev/vd*
        ```
    Pretty cool, eh?

5. Shut down the virtual machine:
    ```
    sudo systemctl poweroff
    ```
#### That was our quick tour of Cockpit.  I hope you enjoyed it!