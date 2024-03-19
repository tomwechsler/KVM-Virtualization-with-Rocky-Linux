We've looked at two ways we can use `libvirt` to administer virtual machines via QEMU/KVM, Virtual Machine Manager (`virt-manager`), and Cockpit.  While these were easy to use, sometimes we need the granularity and power that command-line tools provide.  Fortunately for us, `libvirt` has a set of command-line tools to round out our toolkit.  In this lesson, we will take a look at managing virtual networks using `virsh`.

#### Reference Links

[libvirt: Network XML format](https://libvirt.org/formatnetwork.html)
[libvirt: virsh](https://libvirt.org/manpages/virsh.html#synopsis)

### Managing Virtual Networks Using 'virsh'

The 'virsh' command enables us to manipulate virtual networks and interfaces.

Our DBA team has requested a private virtual network to handle private database traffic.  They would like every new VM to have a connection to this network.

We will create a new network and attach our first virtual machine to it.

#### Creating a Virtual Network Using 'virsh'

Create a new file called `dba_network.xml` with the following contents:
```
<network>
  <name>private</name>
  <bridge name="virbr2"/>
  <ip address="192.168.11.1" netmask="255.255.255.0">
    <dhcp>
      <range start="192.168.11.2" end="192.168.11.254"/>
    </dhcp>
  </ip>
  <ip family="ipv6" address="2002:c0a8:0b01::c0a8:0b01" prefix="64"/>
</network>
```
Save and exit.

Next, we're going to use `virsh net-define` to create a new virtual network using our network configuration:
```
sudo virsh net-define --file dba_network.xml
```
Let's check our work:
```
sudo virsh net-list --all
```
Finally, let's set our network to autostart and start our new network so we can use it:
Set network to autostart:
```
sudo virsh net-autostart private
```
Start the network:
```
sudo virsh net-start private
```
#### Let's check our work, once more:

List networks:
```
sudo virsh net-list --all
```
Get information on `private`:
```
sudo virsh net-info private
```
Take a look at the XML configuration for `private`:
```
sudo virsh net-dumpxml private
```
Display DHCP leases on `private`:
```
sudo virsh net-dhcp-leases private
```
If you'd like to get all of the interfaces' addresses for our virtual machine:
```
sudo virsh domifaddr rocky-9-cli-vm
```

#### Managing Interfaces Using 'virsh'

Now that we have our new 'private' virtual network available let's attach our virtual machine to it.

Let's take a look at the currently assigned network interfaces:
```
sudo virsh domiflist rocky-9-cli-vm
```
Let's attach an interface from our new network to our virtual machine:
```
sudo virsh attach-interface --domain rocky-9-cli-vm --type network --source private --model virtio --config --live
```
Let's take a look at the currently assigned network interfaces again:
```
sudo virsh domiflist rocky-9-cli-vm
```
Let's check the operating system for our new interface:

Log in to the virtual machine using `virsh console` as 'cloud_user':
```
sudo virsh console rocky-9-cli-vm
```
Take a look at the networks on the VM:
```
ip addr
```
You should see the new network (192.168.11.0).

Let's try to ping the virtual network:
```
ping 192.168.11.1
```
You should get a response.

Log out.  Use **CTRL** + **]** to exit the `virsh` console.

#### Congratulations!  You've completed setting up the new DBA network and connection using `virsh`!