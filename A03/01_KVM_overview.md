#### Reference Links

[KVM](https://www.linux-kvm.org/page/Main_Page)  
[Kernel-based Virtual Machine - Wikipedia](https://en.wikipedia.org/wiki/Kernel-based_Virtual_Machine)  
[Guest Support Status - KVM](https://www.linux-kvm.org/page/Guest_Support_Status)  
[Management Tools - KVM](https://www.linux-kvm.org/page/Management_Tools)  

### KVM - Overview

#### What is KVM?

**KVM**

* KVM: Linux kernel module
    * Allows the kernel to function as a type-1 hypervisor
    * Was merged into the kernel mainline as of 2.6.20 (2/2007)
    * Provides the `/dev/kvm` interface
    * Requires hardware virtualization extensions
        * Intel VT-x
            * "vmx" extension
        * AMD-V
            * "svm" extension

#### How Does It Work?

**KVM allows the Linux kernel to function as a type-1 hypervisor**

“KVM (for Kernel-based Virtual Machine) is a full virtualization solution for Linux on x86 hardware containing virtualization extensions (Intel VT or AMD-V). It consists of a loadable kernel module, kvm.ko, that provides the core virtualization infrastructure and a processor specific module, kvm-intel.ko or kvm-amd.ko.

Using KVM, one can run multiple virtual machines running unmodified Linux or Windows images. Each virtual machine has private virtualized hardware: a network card, disk, graphics adapter, etc.

KVM is open source software. The kernel component of KVM is included in mainline Linux, as of 2.6.20. The userspace component of KVM is included in mainline QEMU, as of 1.3.” --KVM Project

**KVM itself does not have tools to manage virtual machines. It only provides access to the hardware acceleration.**

#### The `/dev/kvm` Device

The **KVM** kernel module exposes the `/dev/kvm` device file, which **QEMU** uses to *"talk to"* **KVM**. This allows **QEMU** access to hardware acceleration.

**How do I know if my system supports KVM virtualization?**

A quick test is to check for the existence of `/dev/kvm/`:

```
ls -al /dev/kvm
```

You should see the `/dev/kvm` device:

`crw-rw-rw-. 1 root kvm 10, 232 Apr 25 17:57 /dev/kvm`

You can also check for Intel VT-x (vmx) or AMD-V support (svm):

```
egrep -c "vmx|svm" /proc/cpuinfo
```

You should see a number of one or greater. If this returns a result of zero, then you do not have hardware virtualization support.

You can also check for the existence of the `kvm` kernel modules:

```
lsmod | grep kvm
```

On an Intel system, you would see `kvm_intel` and `kvm`.