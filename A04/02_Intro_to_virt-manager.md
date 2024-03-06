#### Reference Links

- [Virtual Machine Manager Home](https://virt-manager.org/)  
- [Virtual Machine Manager - Wikipedia](https://en.wikipedia.org/wiki/Virtual_Machine_Manager)  
- [How to run virtual machines with virt-manager - Fedora Magazine](https://fedoramagazine.org/full-virtualization-system-on-fedora-workstation-30/)  
- [Getting Started with Virtual Machine Manager Red Hat Enterprise Linux 7 \| Red Hat](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/virtualization_getting_started_guide/chap-virtualization_manager-introduction)  

### Introduction to 'virt-manager'

#### What is 'virt-manager'?

From the [Virtual Machine Manager website:](https://virt-manager.org/)

"The `virt-manager` application is a desktop user interface for managing virtual machines through `libvirt`. It primarily targets KVM VMs, but also manages Xen and LXC (Linux Containers). It presents a summary view of running domains, their live performance, and resource utilization statistics. Wizards enable the creation of new domains, and configuration and adjustment of a domain’s resource allocation and virtual hardware. An embedded VNC and SPICE client viewer presents a full graphical console to the guest domain."

#### Virtual Machine Manager (virt-manager) functionality:

- Creates, edits, starts, and stops guest VMs
- Provides console access for guest VMs
- Provides operating metrics for guest VMs
- Displays the current status of virtualization hosts and guest VMs
- Provides a *choice of hypervisors*
- Supports LXC containers

#### How does 'virt-manager' work?

Virtual Machine Manager runs in a user space and leverages `libvirt` to access the underlying hypervisor(s).  Because of this, `virt-manager` doesn't have to know how to ***"talk to"*** each particular hypervisor, it only has to communicate with `libvirt` via the `libvirtd` service on each host.

#### Linux Distribution Support

**The `virt-manager` package is available for:**

- Arch Linux
- CentOS
- Debian
- Fedora
- FreeBSD
    - via Ports collection
- Frugalware
- Gentoo
- Mandriva Linux
- NetBSD
    - via pkgsrc
- OpenBSD
    - via Ports collection
- openSUSE
- Red Hat Enterprise Linux
    - Deprecated in 8, but still around
    - Web Console / Cockpit will be the replacement
    - Still can't do everything yet
- Scientific Linux
- Trisquel
- TrueOS
- Ubuntu
- Void Linux