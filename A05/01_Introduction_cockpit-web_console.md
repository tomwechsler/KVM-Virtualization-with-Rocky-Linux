In this lesson, we will take a look at Cockpit/Web Console, which is a web application that we can use to manage not only the host but the guest virtual machines that run on QEMU/KVM hypervisors.

#### Reference Links

- [Cockpit Project](https://cockpit-project.org/)
- [Cockpit Guide](https://cockpit-project.org/guide/latest/)
- [Cockpit - Virtual Machines](https://cockpit-project.org/guide/latest/feature-virtualmachines)
- [Cockpit - Multiple Hypervisors](https://cockpit-project.org/guide/172/feature-machines.html)
- [Is Cockpit Secure? — Cockpit Project](https://cockpit-project.org/blog/is-cockpit-secure.html)

### Introduction to Cockpit / Web Console

#### What is Cockpit?

**From the Cockpit Project website:**

"The easy-to-use, integrated, glanceable, and open web-based interface for your servers."

***Cockpit and Web Console are the same thing.  Cockpit is the upstream project that feeds Web Console in the RHEL offering.***

#### Cockpit Functionality

**Cockpit/Web Console can:**

- Create, edit, start and stop guest VMs
- Provide console access for guest VMs
- Provide basic operating metrics for guest VMs
- Display the current status of guest VMs
- Provide a choice of hypervisors:
  - SSH connection/keys
  - Manual configuration
- Inspect and change network settings
- Configure a firewall
- Manage storage (including RAID and LUKS partitions)
- Download and run containers
- Browse and search system logs
- Inspect a system’s hardware
- Upgrade software
- Keep tabs on performance

**For RHEL distributions, Cockpit is the way forward.  Virtual Machine Manager (only for RHEL) is deprecated.**

#### How Does it Work?

**How does Cockpit work?**

***Cockpit runs in the user space and leverages a plugin to use libvirt to access the underlying hypervisor.  Because of this, Cockpit doesn't have to know how to "talk to" the hypervisor, it only has to communicate with libvirt via the libvirtd service.***

**From the [Cockpit Project](https://cockpit-project.org/guide/latest/feature-virtualmachines) website:**

"Cockpit can manage virtual machines running on the host. These can be accessed from menu via *Virtual Machines*.

Primary datasource is *QEMU / Libvirt*, access to *Libvirtd* is wrapped either by the *virsh* tool or *libvirt D-Bus API bindings*, depending on if the latter is installed on the system."

**Cockpit is a web application.**

Cockpit can be accessed via a web browser on port ***9090*** on the host machine.  Recommended browsers include Mozilla Firefox, Google Chrome, and Microsoft Edge.

#### Linux Distribution Support

**The Cockpit package is available for:**

- Fedora
- RHEL
  - Web Console
- CoreOS
- Atomic
- CentOS
- Debian
- Ubuntu
- Clear Linux
- Arch Linux