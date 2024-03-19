We've looked at two ways we can use `libvirt` to administer virtual machines via QEMU/KVM, Virtual Machine Manager (`virt-manager`), and Cockpit.  While these were easy to use, sometimes we need the granularity and power that command-line tools provide.  Fortunately for us, `libvirt` has a set of command-line tools to round out our toolkit.  In this lesson, we will take a look at how to use the `virt-clone` command to clone a virtual machine.

#### Reference Links

[virt-clone(1) - Linux man page](https://linux.die.net/man/1/virt-clone)

### Using 'virt-clone' to Clone a Virtual Machine

In this lesson, we're going to clone our `rocky-9-cli-vm` and patch it so we can turn it into a template to for new CentOS 7 VMs for our DBA team.

#### Clone a virtual machine

Before we clone our virtual machine, we need to shut it down.

1. Check the status of the `rocky-9-cli-vm` guest:
    ```
    sudo virsh list --all
    ```
2. Shut the `rocky-9-cli-vm` virtual machine down (if it is running):
    ```
    sudo virsh shutdown rocky-9-cli-vm
    ```
3. Confirm that the virtual machine is shut down:
    ```
    sudo virsh list --all
    ```

#### Start the clone

We're going to clone our virtual machine so we can turn the clone into a template.  This preserves our original VM in case something goes wrong during the template phase of our work.

Clone our `rocky-9-cli-vm` guest, with the clone name of `rocky-9-db-template`, including *all* the virtual disks:
```
sudo virt-clone --original=rocky-9-cli-vm --name=rocky-9-db-template --auto-clone
```

#### Patch the cloned virtual machine

1. Start the new clone:
    ```
    sudo virsh start rocky-9-db-template
    ```
2. Log in to the virtual machine using `virsh console` as `tom`:
    ```
    sudo virsh console rocky-9-db-template
    ```
3. Patch the VM:
    ```
    sudo yum -y update
    ```
4. When the patching is complete, log out.  Use **CTRL** + **]** to exit the `virsh` console.

#### Shut down the cloned virtual machine

1. Shut the `rocky-9-db-template` virtual machine down:
    ```
    sudo virsh shutdown rocky-9-db-template
    ```
2. Check the status of all guests:
    ```
    sudo virsh list --all
    ```

**We now have a patched virtual machine that we can create a template from!**