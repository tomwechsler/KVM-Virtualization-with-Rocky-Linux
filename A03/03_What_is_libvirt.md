#### Reference Links

[libvirt: The virtualization API](https://libvirt.org/)  
[libvirt: KVM/QEMU hypervisor driver](https://libvirt.org/drvqemu.html)  
[libvirt: Internal drivers](https://libvirt.org/drivers.html)  
[libvirt: Supported host platforms](https://libvirt.org/platforms.html)  
[libvirt: Applications using libvirt](https://libvirt.org/apps.html)  
[Libvirt Wiki](https://wiki.libvirt.org/page/Main_Page)  
[libvirt: Downloads](https://libvirt.org/downloads.html)  
[libvirt: Documents](https://libvirt.org/docs.html)  
[FAQ - Libvirt Wiki](https://wiki.libvirt.org/page/FAQ)  
[Cockpit - Virtual Machines](https://cockpit-project.org/)  
[Management Tools - KVM](https://www.linux-kvm.org/page/Management_Tools)  

### What is libvirt?

**From [the libvirt project:](https://libvirt.org/)**

**libvirt:**
- is a toolkit to manage virtualization platforms
- is accessible from C, Python, Perl, Java and more
- is licensed under open source licenses
- supports KVM, QEMU, Xen, Virtuozzo, VMWare ESX, LXC, BHyve and more
- targets Linux, FreeBSD, Windows and OS-X
- is used by many applications

#### How does libvirt work?

**From [the libvirt project:](https://libvirt.org/)**

"Libvirt is collection of software that provides a convenient way to manage virtual machines and other virtualization functionality, such as storage and network interface management. These software pieces include an API library, a daemon (libvirtd), and a command line utility (virsh).

An primary goal of libvirt is to provide a single way to manage multiple different virtualization providers/hypervisors. For example, the command 'virsh list --all' can be used to list the existing virtual machines for any supported hypervisor (KVM, Xen, VMWare ESX, etc.) No need to learn the hypervisor specific tools!"

#### libvirt - Virtualization Platform Support

- Linux
- Windows
- macOS
- FreeBSD

#### libvirt - Hypervisor Support

The **hypervisor drivers** currently supported by libvirt are:

- LXC 
  - Linux Containers
- OpenVZ
- **QEMU**
- Test
  - Used for testing
- **VirtualBox**
- VMware ESX
- VMware Workstation/Player
- Xen
- Microsoft Hyper-V
- Virtuozzo
- Bhyve - The BSD Hypervisor

**We will take a look at these end-user management tools supported by libvirt on Linux:**

- Virtual Machine Manager - 'virt-manager'
- Cockpit / Web Console
  - Via [plugin](https://cockpit-project.org/)
- CLI - 'virsh'
- oVirt / RHEV