We've looked at two ways we can use `libvirt` to administer virtual machines via QEMU/KVM, Virtual Machine Manager (virt-manager) and Cockpit.  While these were easy to use, sometimes we need the *granularity and power* that command-line tools provide.  Fortunately for us, `libvirt` has a set of command-line tools to round out our toolkit.  In this lesson, we will take a look at the CLI options provided by `libvirt`.

#### Reference Links

[libvirt: virsh](https://libvirt.org/manpages/virsh.html)

### Introduction to Linux CLI Utilities

#### What is 'virsh'?

**From the 'libvirt' website:**

“The virsh program is the *main interface* for managing virsh guest domains.”

#### 'virsh' Functionality

**'virsh' can:**

- Create, edit, start, and stop guest VMs
- Provide console access for guest VMs
- Provide basic operating metrics for guest VMs
- Display the current status of guest VMs
- Manage virtual networks
- Manage virtual storage
- And *more*

**Additional CLI utilities:**

- `virt-install`
- `virt-sysprep`
- `virt-clone`
- `virt-v2v`

**Why use CLI utilities?**

- **More control** than other methods
- Enable scripting/automation
- No GUI/web browser required:
  - Less overhead
  - Easier to access the CLI
- Many management utilities leverage 'virsh':
  - Easier to troubleshoot issues

#### How does it work?

`virsh` runs in user space and leverages `libvirt` to access the underlying hypervisor. Because of this, `virsh` doesn’t have to know how to “talk to” the hypervisor — it only has to communicate with `libvirt` via the `libvirtd` service.

**Interacting with virsh:**

You can use `virsh` in one of two ways:

- From the Linux shell
- As its own interactive shell:
  - The virsh shell

**How does the Cockpit plugin work?**

“Access to Libvirtd is wrapped either by the virsh tool or libvirt D-Bus API bindings, depending if the latter is installed on the system.”

— Cockpit Project

#### Let's get to working with 'virsh'!