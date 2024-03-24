We've looked at two ways we can use `libvirt` to administer virtual machines via QEMU/KVM, Virtual Machine Manager (`virt-manager`), and Cockpit.  While these were easy to use, sometimes we need the granularity and power that command-line tools provide.  Fortunately for us, `libvirt` has a set of command-line tools to round out our toolkit.  In this lesson, we will take a look at how to use the `virt-sysprep` utility to turn our clone of our rocky 7 virtual machine into a template that we can clone database servers from.  We will then clone our first database server from the template!

#### Reference Links

[virt-sysprep](http://libguestfs.org/virt-sysprep.1.html)
[virt-clone(1) - Linux man page](https://linux.die.net/man/1/virt-clone)

### Using 'virt-sysprep' to Create a Virtual Machine Template

In this lesson, we will use the `virt-sysprep` command to turn the 'rocky-9-db-template' clone into a virtual machine template.

#### Create a virtual machine template using `virt-sysprep`

1. Before we proceed, we want to make sure the `rocky-9-db-template` guest is shut down:
    ```
    sudo virsh list --all
    ```
2. Confirm the guest shuts down.

3. Let's take a look at the operations that we will perform by default:
    ```
    sudo virt-sysprep --list-operations | more
    ```
4. Execute the `virt-sysprep` operation:
    ```
    sudo virt-sysprep -d rocky-9-db-template --enable user-account --keep-user-accounts tom --update
    ```
We are keeping the `tom` account and will check for updates when we execute and apply them if necessary.  We just patched the template, so we probably won't have any updates, but if we execute this operation in the future, we probably will.

#### Clone the virtual machine template to a new guest using `virt-clone`

Now, let's clone our new `rocky-9-vm-template` to `rocky-9-db1-vm`.  This will give us our new database server.

Clone our `rocky-9-db-template` guest, with the clone name of `rocky-9-db1-vm`, including *all* the virtual disks:
```
sudo virt-clone --original=rocky-9-db-template --name=rocky-9-db1-vm --auto-clone
```

#### Take a look at our cloned machine

Now that we have our new database VM, let's take a look at it.

1. Start the new clone:
    ```
    sudo virsh start rocky-9-db1-vm
    ```
2. Let's get information on the domain:
    ```
    sudo virsh dominfo rocky-9-db1-vm
    ```
3. List the network interfaces:
    ```
    sudo virsh domiflist rocky-9-db1-vm
    ```
4. List the block devices:
    ```
    sudo virsh domblklist rocky-9-db1-vm
    ```
5. Log in to the virtual machine using `virsh console` as `tom`:
    ```
    sudo virsh console rocky-9-db1-vm
    ```
6. Look around and check out the network, storage, hostname, etc.  
7. When you're done, log out.  Use **CTRL** + **]** to exit the `virsh` console.

Congratulations!  You now have a database virtual machine template and a new database server from your template.  You can now deploy fresh database servers as quickly as you can clone and configure them!