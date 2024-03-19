We've looked at two ways we can use `libvirt` to administer virtual machines via QEMU/KVM, Virtual Machine Manager (`virt-manager`), and Cockpit.  While these were easy to use, sometimes we need the granularity and power that command-line tools provide.  Fortunately for us, `libvirt` has a set of command-line tools to round out our toolkit.  In this lesson, we will use the `virt-install` command to create a new Rocky 9 virtual machine.

#### Reference Links

[libvirt: virsh](https://libvirt.org/manpages/virsh.html#synopsis)
[Rocky 9 Mirror](https://download.rockylinux.org/pub/rocky/9/BaseOS/x86_64/os/)

#### Using `virt-install` to Create a Virtual Machine

Our DBA team has put in a request for a new Rocky 9 virtual machine.  We want to create a Rocky 9 template to deploy this virtual machine, so we can deploy *more* VMs later if we need to.

We're going to install using a *serial console* and a *text-based installer*, installing directly from the *Rocky 9 mirror*.

1. Let's launch a Rocky 9 installation using `virt-install`:
    ```
    sudo virt-install --name=rocky-9-cli-vm --vcpus=1 --memory=2048 --location https://mirror.puzzle.ch/rockylinux/9.3/AppStream/x86_64/os/ --disk pool=Virtual_Machines,size=10,bus=virtio --os-type linux --os-variant=rocky9 --network network='default',model=virtio --extra-args='console=ttyS0,115200n8 serial' --nographics
    ```
2. Set the values for `vcpus` and `memory` based on your system's resources.  The ones above are more than adequate for a *Minimal* Rocky 9 installation:
    - When we get to the *VNC* menu, choose 'Use text mode', choice number 2.
    - When you get to the 'Installation' menu, complete the sections with a **[!]** next to them.
      - You can leave the ones with an **[x]** next to them as-is if you want

3. Start the installation.  Once the installation is complete, press **return** to exit.  The virtual machine will reboot.

4. When the VM comes back up, log in as `tom`.  Patch the VM:
    ```
    sudo yum -y update
    ```
5. When patching is complete, log out of the system.  Use **CTRL**+**]** to exit the `virsh` console.

#### Check out the New Virtual Machine using 'virsh'

- Let's take a look for our new virtual machine in our list of existing virtual machines:
    ```
    sudo virsh list --all
    ```
- Let's take a look for our new virtual disk in the `Virtual_Machines` storage pool:
    ```
    sudo virsh vol-list Virtual_Machines
    ```

#### Check out the new virtual machine using Cockpit

Log in to the Cockpit web console at `http://YOUR_COMPUTER_IP:9090` as `tom`.  On the left-hand side, click on **Virtual Machines** to go to that pane.  Click on your new virtual machine and check out the details.  Check out the text console.

#### Congratulations on your new virtual machine, created with `virsh`!