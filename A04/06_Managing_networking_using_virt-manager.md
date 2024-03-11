The Virtual Machine Manager (virt-manager) is a desktop virtual machine manager that gives users a graphical user interface to manage virtual machines.  In this lesson, we will walk through configuring an additional network interface and will add a new virtual network and connect to it.

### Managing Networking Using 'virt-manager'

#### Adding a virtual network interface

Our webserver virtual machine has a network connection to the 'default' NAT network.  This allows the server to access the outside world.  Our Web Developer needs to be able to access the web server via the `public` internal network. Let's add a network interface that connects directly to the 'public' internal network.

1.  Show the available networks on the host:
    ```
    ip addr
    ```
2. Display the network configuration files (in older Versions: /etc/sysconfig/network-scripts):
    ```
    ls -al /etc/NetworkManager/system-connections
    ```
3. Go to 'Virtual Hardware Details'.
4. In the bottom left-hand corner, click on the **Add Hardware** button.
5. On the left-hand side of the *Add New Virtual Hardware* window, click on **Network**.
6. We're going to add a 'macvtap' device on the physical adapter we want to use:
    - *Source mode:* Bridge
    - Leave MAC address as-is
    - *Device model:* virtio
7. Click on **Finish**.
8. Show the available networks on the host:
    ```
    ip addr
    ```
9. If you have another host on the network, try to SSH as 'tom' to the new IP:
    ```
    ssh tom@IP_ADDRESS
    ```
10. If you have another host on the network, try to connect to it with a web browser at the new IP.
11. We need to add a firewall rule to allow HTTP traffic:
    ```
    sudo firewall-cmd --zone=public --add-service=http --permanent
    ```
    ```
    sudo firewall-cmd --reload
    ```
12. Now that our firewall is configured to allow HTTP traffic, try to connect to the new IP address again from a web browser.

#### Adding / connecting to a virtual network

As we start to add new virtual servers, we want to monitor them, and as part of that, we want to create a management network on our host.  Let's create a new virtual network and connect our VM to it.

1. Close the window for the `rocky-9-vm` VM.
2. Click on the **QEMU/KVM** hypervisor.
3. Right-click on this, select **Details**.
4. Choose **Virtual Networks**. You will see the *default* network
5. Click on the **+** button in the lower left-hand corner.
6. In the 'Create a new virtual network' wizard, set the following:
    - *Name:* Management_Net
    - - *Mode:* Isolated
      - *IPv4 configuration:* KEEP DEFAULTS
      - *IPv6 configuration:* LEAVE DISABLED
      - *DNS domain name:* KEEP DEFAULTS
7. Click on **Finish**.
8. Open the VM console window.
9. Show the available networks on the host:
    ```
    ip addr
    ```
10. Let's add a new interface to connect to the new 'Management_Net' network.
11. Go to 'Virtual Hardware Details'.
12. In the bottom left-hand corner, click on the **Add Hardware** button.
13. On the left-hand side of the *Add New Virtual Hardware* window, click on **Network**.
14. We're going to add a `macvtap` device on the physical adapter we want to use:
    - *Network source:* Virtual network 'Management_Net' : Isolated network
    - Leave MAC address as-is
    - *Device model:* virtio
15. Click on **Finish**.
16. Go back to the console window.
17. Show the available networks on the host.:
    ```
    ip addr
    ```
We now have a connection to the isolated `Management_Net` network!