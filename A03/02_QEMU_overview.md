#### Reference Links

[QEMU](https://www.qemu.org/)  
[QEMU documentation - QEMU](https://www.qemu.org/documentation/)  
[QEMU System Emulation User’s Guide — QEMU](https://www.qemu.org/docs/master/system/index.html)  
[Understanding QEMU devices - QEMU](https://www.qemu.org/2018/02/09/understanding-qemu-devices/)  

### QEMU - Overview

#### What is QEMU?

**From qemu.org:**  "QEMU is a generic and open source machine emulator and virtualizer."

QEMU is a type-2 hypervisor that runs in user space. When used with KVM, it accelerates the performance of a QEMU guest and the combination becomes a type-1 hypervisor.

#### How does QEMU do this?

QEMU is a *user space* program that has two modes:

- Emulation (software)
  - Slower as software must perform hardware emulation
  - Type-2 Hypervisor
- Virtualization (software+hardware)
  - Hardware acceleration using KVM
  - Type-1 Hypervisor

QEMU creates one process for every VM, and a thread for each vCPU. These run in the Linux scheduler. A virtual machine is a collection of processes running on the host.

QEMU interacts with KVM in two ways:

- Memory mapped pages
- The `/dev/kvm` device file
  - System Level: Whole KVM subsystem
  - VM Level: One specific VM
  - vCPU Level: One specific vCPU

#### QEMU - Devices

**QEMU and the `/dev/kvm` interface combine to provide the resources the guest operating system requires:**

**Key hardware:**

- Virtual CPU(s)
- Virtual Memory
- Virtual Disk(s)
- Virtual Networking

**Mapped physical resources:**

- Networking:
  - IP
  - Storage
- GPU(s)

**Other resources:**

- Virtual media:
  - CD/DVD/ISO images
- USB
- Sound
- Video

#### QEMU - Paravirtualization

**QEMU/KVM provides paravirtualization support**

**Two parts:**

- Provides a software interface that looks like hardware
- VirtIO drivers for the guest operating system

**Offloads some “challenging” processes from the guest to the host:**

**Uses the VirtIO API**

- Ethernet
- Storage
- Memory balloon device
- Display

#### Disk Image Formats

**Read-Write**

- QEMU - Copy-On-Write (.qcow2, .qed, .qcow, .cow)
- VirtualBox Virtual Disk Image (.vdi)
- Virtual PC Virtual Hard Disk (.vhd)
- Virtual VFAT
  - A virtual drive with a FAT filesystem
  - Quick way to share files between guest and host
- VMware Virtual Machine Disk (.vmdk)
- Raw images (.img)
  - These contain sector-by-sector disk contents
- Bootable CD/DVD Images (.iso)

**Read-Only**

- macOS Universal Disk Image Format (.dmg)
- Bochs
- Linux cloop
  - Compressed image format
- Parallels disk image (.hdd, .hds)

#### QEMU Emulation Support

- IA-32 (x86) PCs
- x86-64 PCs
- MIPS64 Release 6 and earlier variants
- Sun's SPARC sun4m
- Sun's SPARC sun4u
- ARM development boards (Integrator/CP and Versatile/PB)
- SH4 SHIX board
- PowerPC (PReP and Power Macintosh)
- ETRAX CRIS
- MicroBlaze
- RISC-V