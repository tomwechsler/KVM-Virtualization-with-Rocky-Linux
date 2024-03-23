In this lesson, we're going to take a look at oVirt, an enterprise-class virtualization platform based on KVM/QEMU/libvirt.

#### Reference Links

[oVirt \| Home](https://www.ovirt.org/)
[oVirt \| System Requirements](https://www.ovirt.org/documentation/install-guide/chap-System_Requirements.html)
[oVirt \| Documentation](https://www.ovirt.org/documentation/)
[oVirt \| Admin Guide](https://www.ovirt.org/documentation/administration_guide/index.html)
[oVirt \| oVirt Installation Guide](https://www.ovirt.org/documentation/install-guide/Installation_Guide.html)

### Introduction to oVirt

#### What is oVirt?

**From the oVirt website:**

“oVirt is an open-source distributed virtualization solution, designed to manage your entire enterprise infrastructure. oVirt uses the trusted KVM hypervisor and is built upon several other community projects, including libvirt, Gluster, PatternFly, and Ansible.”

What's included?

- Rich web-based user interfaces for both admin and non-admin users
- Integrated management of hosts, storage, and network configuration
- Live migration of virtual machines and disks between hosts and storage
- High availability of virtual machines in the event of a host failure

#### oVirt is:

- Built on `libvirt/QEMU/KVM`
- The upstream project of RHEV

#### oVirt Functionality

**ovirt can:**

- Create, edit, start, and stop guest VMs
- Move guest VMs between hosts
- Provide console access for guest VMs
- Provide operating metrics for guest VMs
- Display the current status of guest VMs
- Manage virtual networks
- Manage virtual storage
- Provide disaster recovery
- More

#### How Does oVirt Work?

##### oVirt is made up of two components:

oVirt engine:

- Standalone or self-hosted
- Java backend
- GWT web toolkit frontend
- WildFly (JBoss) application server
- PostgreSQL database

oVirt node:

- RHEL/CentOS (predominantly)
- QEMU/KVM/libvirt
- VDSM:
    - Node management API
- Gluster/PatternFly/Ansible
- Nodes can be ***clustered***

##### oVirt supports many storage solutions:

NFS
iSCSI
GlusterFS
POSIX-compliant filesystem
Fibre Channel

#### Next, we’re going to stand up an oVirt environment on Rocky Linux 9.