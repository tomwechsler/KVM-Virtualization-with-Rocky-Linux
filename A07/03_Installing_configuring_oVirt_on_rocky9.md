Now that we have a Rocky 9 server up and running, we can get to setting up oVirt!  In this lesson, we are going to install oVirt on Rocky 9, using the command line installer.

#### Reference Links

**oVirt:**
[Installing oVirt as a self-hosted engine using the command line \| oVirt](https://www.ovirt.org/documentation/installing_ovirt_as_a_self-hosted_engine_using_the_command_line/)
[oVirt Installation Guide \| oVirt](https://www.ovirt.org/documentation/install-guide/Installation_Guide.html)

**DNS:**
[Chapter 15. DNS Servers Red Hat Enterprise Linux 7 \| Red Hat Customer Portal](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/networking_guide/ch-dns_servers)

### Installing and Configuring oVirt on Rocky 9

#### **NOTE:** Installation requires that you have both forward and reverse DNS properly configured for both your hosted engine appliance and your host!

If you would like to set up a DNS server on your Rocky 9 server, you can. Configuring a DNS server is outside the scope of this course, but it is not difficult, and I have provided some resources in the Reference Links above to get you started.

It is possible you can "fake" DNS using the `/etc/hosts` file on any machines, virtual or real, you are using in your test environment as well.  This is going to be a bit more work to maintain than a centralized DNS.

I am utilizing my pre-existing DNS and DHCP set up in my home lab environment, which runs on Linux, outside of the Rocky 9 servers I am using for these lessons.  I have forward and reverse DNS configured for the servers and the hosted engine, as well as a DHCP reservation for the hosted engine.

#### Virtualizing with oVirt and NFS

We work for a large retail/e-commerce company that is deploying a new warehouse management system.  The company wants to minimize the amount of on-site equipment in the warehouses, and the decision has been made to virtualize the warehouse management systems, using oVirt, in a centralized hosting facility.  We will be using NFS storage for this deployment, on our first host, for now.  Let's deploy our first oVirt host!

#### NFS Storage Configuration

1. Create the `kvm` group:
    ```
    sudo groupadd kvm -g 36
    ```
2. Create the `vdsm` user and assign it to the group `kvm`:
    ```
    sudo useradd vdsm -u 36 -g 36
    ```
3. Change ownership of the NFS storage directories:
    ```
    sudo chown -R 36:36 /media/ISO
    ```
    ```
    sudo chown -R 36:36 /media/VM_Store
    ```
    ```
    sudo chown -R 36:36 /media/HE_Store
    ```
    ```
    sudo chown -R 36:36 /media/Export_Store
    ```
4. Change the NFS storage directory permissions to `0755`:
    ```
    sudo chmod 0755 /media/ISO
    ```
    ```
    sudo chmod 0755 /media/VM_Store
    ```
    ```
    sudo chmod 0755 /media/HE_Store
    ```
    ```
    sudo chmod 0755 /media/Export_Store
    ```

#### Install and Deploy the oVirt Hosted Engine

***Reminder: Installation requires that you have both forward and reverse DNS properly configured for both your hosted engine appliance and your host!***

[Installing on RHEL 9.0 or derivatives \| oVirt](https://www.ovirt.org/download/install_on_rhel.html)

1. Create the oVirt repository:
    ```
    sudo vim /etc/yum.repos.d/CentOS-Stream-Extras-common.repo
    ```
    Add the following to the file:
    ```
    [c9s-extras-common]
    name=CentOS Stream $releasever - Extras packages
    metalink=https://mirrors.centos.org/metalink?repo=centos-extras-sig-extras-common-$stream&arch=$basearch&protocol=https,http
    gpgkey=https://www.centos.org/keys/RPM-GPG-KEY-CentOS-SIG-Extras
    gpgcheck=1
    repo_gpgcheck=0
    metadata_expire=6h
    countme=1
    enabled=1
    ```
    Save and exit the file.

2. Add "9-Stream":
    ```
    echo "9-stream" | sudo tee -a /etc/yum/vars/stream
    ```
3. Synchronize the system with the CentOS Stream repositories:
    ```
    sudo dnf -y distro-sync
    ```
4. Install the oVirt release:
    ```
    sudo dnf install -y centos-release-ovirt45
    ```
5. Install the oVirt openswitch package:
    ```
    sudo dnf install -y ovirt-openvswitch
    ```
6. Install some additional packages:
    ```
    dnf install -y \
    https://kojihub.stream.centos.org/kojifiles/packages/nmstate/2.0.0/2.el9/noarch/nmstate-plugin-ovsdb-2.0.0-2.el9.noarch.rpm \
    https://kojihub.stream.centos.org/kojifiles/packages/nmstate/2.0.0/2.el9/noarch/python3-libnmstate-2.0.0-2.el9.noarch.rpm
    ```
7. Install the self-hosted engine setup package:
    ```
    sudo dnf -y install ovirt-hosted-engine-setup
    ```
8. Execute the deployment script:
    ```
    sudo hosted-engine --deploy --4
    ```
You will need to provide answers in your configuration script.  Most of these will be the default, but some will depend on your environment.  Note that the self-hosted engine installation is automated using Ansible.

#### Connect to the oVirt Engine Web Interface

In a web browser, navigate to `https://manager-fqdn`, replacing `manager-fqdn` with the FQDN for your oVirt hosted engine.

We now have a working oVirt installation!