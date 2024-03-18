We've looked at two ways we can use `libvirt` to administer virtual machines via QEMU/KVM, Virtual Machine Manager (`virt-manager`), and Cockpit.  While these were easy to use, sometimes we need the granularity and power that command-line tools provide.  Fortunately for us, `libvirt` has a set of command-line tools to round out our toolkit.  In this lesson, we will take a look at the `virsh` shell, the core utility for managing virtual machines from the CLI using `libvirt`.

#### Reference Links

[libvirt: virsh](https://libvirt.org/manpages/virsh.html#synopsis)
[libvirt: Domain XML format](https://libvirt.org/formatdomain.html)

### Introduction to the 'virsh' Shell

Before we start working with virtual machines, let's take a look at the `virsh` shell and our virtualization environment.

#### Using the 'virsh' shell

The `virsh` commands can either be executed from within the `virsh` shell or independently from the Linux shell.  We're going to take a look at executing commands from within the `virsh` shell.

#### Basic 'virsh' commands

1. First, enter the `virsh` shell:
```
sudo virsh
```
2. Demonstrate basic `virsh` commands within the `virsh` shell**

    - Help information - 'help'
    - Version information - 'version'
    - List guest domains - 'list --all'
    - start / shutdown / reboot / suspend / resume
    - Show CPU information - 'vcpuinfo rocky-9-cockpit-vm' / 'vcpucount rocky-9-cockpit-vm'
    - Show VNC information - 'vncdisplay rocky-9-cockpit-vm'
    - 'exit' /  'quit'

#### Domain monitoring commands

**Information:**

- Get basic domain information — `dominfo rocky-9-cockpit-vm`
- Get state information on a guest domain —`domstate rocky-9-cockpit-vm`
- Get block device size — `domblkinfo rocky-9-cockpit-vm --all`
- Get network interface list — `domiflist rocky-9-cockpit-vm`
- Convert a domain name or UUID to domain id — `domid rocky-9-cockpit-vm`
- Convert a guest domain id or UUID to a guest domain name — `domname ID`
- Convert a guest domain name or id to guest domain UUID — `domuuid ID`
- Output guest domain information as XML — `dumpxml ID`

**Statistics:**

- Get block device stats — `domblkstat rocky-9-cockpit-vm`
- Get network interface stats — `domifstat --domain rocky-9-cockpit-vm vnet0`
- Get memory statistics — `dommemstat rocky-9-cockpit-vm`

#### Host and hypervisor commands

- Display the capabilities of a hypervisor — `capabilities`
- Display the hostname of the hypervisor host — `hostname`
- Display basic information about the node — `nodeinfo`
- Display the hypervisor URI — `uri`

#### That's it!  Pretty easy to use, huh?