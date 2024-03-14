We've looked at two ways we can use `libvirt` to administer virtual machines via QEMU/KVM, Virtual Machine Manager (virt-manager) and Cockpit.  While these were easy to use, sometimes we need the granularity and power that command-line tools provide.  Fortunately for us, `libvirt` has a set of command-line tools to round out our toolkit.  In this lesson, we will take a look at installing the `libvirt` CLI utilities.

#### Reference Links

[libvirt: virsh](https://libvirt.org/manpages/virsh.html#synopsis)

### Installing the CLI Utilities on Rocky 9

We've been asked by the DBA team to set up a barebones virtualization environment so they can do some test database migrations away from CentOS 7.  We've settled on a minimal Rocky 9 server with Cockpit to provide easy access for the DBA team.  We will work "under the hood" with `virsh` to do the heavy lifting.

Since we already have a minimal Rocky 9 server with Cockpit installed and configured, we're good to go.  Now, we're going to install the packages we need for CLI administration and will test the installation to make sure it is working.

#### Install packages using `dnf`/`yum`

1. Install the `libvirt` utilities and dependencies:
    ```
    sudo dnf -y install qemu-kvm libvirt libguestfs-tools virt-install virt-v2v
    ```
2. These may already be installed from our earlier work on the server.

#### Confirm libvirtd is Enabled and Running

Check the status of the `libvirtd` service:
```
sudo systemctl status libvirtd
```

#### Check the Version of `virsh`

Execute the following:
```
virsh version
```

#### Validate the Host

Let's check our host and make sure it meets the requirement for virtualization:
```
virt-host-validate
```

#### Check Bridged Networking

1. We need to confirm that we have one or more bridged networks in place:
    ```
    sudo virsh net-list
    ```
    We should see the `default` network.

2. For more information on the `default` network, we can look at the XML:
```
sudo virsh net-dumpxml default
```

#### List existing virtual machines

Let's take a look at our list of existing virtual machines:
```
sudo virsh list --all
```

#### List existing storage pools

- Let's take a look at our list of existing storage pools:
```
sudo virsh pool-list
```
- Let's take a look at a list of volumes in the `Virtual_Machines` storage pool:
```
sudo virsh vol-list Virtual_Machines
```

#### The 'libvirt' CLI utilities are now installed!