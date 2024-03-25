We've looked at two ways we can use `libvirt` to administer virtual machines via QEMU/KVM, Virtual Machine Manager (`virt-manager`), and Cockpit.  While these were easy to use, sometimes we need the granularity and power that command-line tools provide.  Fortunately for us, `libvirt` has a set of command-line tools to round out our toolkit.  In this lesson, we will take a look at managing virtual storage using `virsh`.

#### Reference Links

[libvirt: Storage Management](https://libvirt.org/storage.html)
[libvirt: virsh](https://libvirt.org/manpages/virsh.html#synopsis)

### Managing Virtual Storage Using `virsh`

The `virsh` command enables us to manipulate storage pools, volumes, and snapshots.

Our DB team has requested five additional storage volumes for the new Rocky 9 database VM.  We want to create these in a new storage pool so we can manage and track the DB team's storage resources in one place.  We will also take a snapshot of our virtual machine after we add the storage, in case we need to roll back.

#### Managing Storage Pools Using `virsh`

We are going to create a new storage pool for the DB team.  

1. Let's take a look at our current inventory of storage pools:
    ```
    sudo virsh pool-list --all
    ```
2. Let's create a new storage pool for the DB team:
    ```
    sudo virsh pool-define-as DB_Team dir - - - - "/db/dbstorage"
    ```
3. Let's take a look at our current inventory of storage pools:
    ```
    sudo virsh pool-list --all
    ```
4. Use `virsh pool-build` to build the storage pool:
    ```
    sudo virsh pool-build DB_Team
    ```
5. Confirm we have what we want in `/db`:
    ```
    sudo ls -la /db
    ```
6. Let's set our new storage pool to autostart:
    ```
    sudo virsh pool-autostart DB_Team
    ```
7. We will need to start the storage pool:
    ```
    sudo virsh pool-start DB_Team
    ```
8. Now, let's take a look at our current inventory of storage pools:

9. List storage pools:
    ```
    sudo virsh pool-list --all
    ```
10. Get pool information for 'DB_Team':
    ```
    sudo virsh pool-info DB_Team
    ```
Now the DB team has a place to keep their VMs and storage volumes.

#### Managing Volumes Using 'virsh'

Next, we will need to create five new storage volumes, attach them to the virtual machine, and confirm they are accessible to the operating system.

1. Use `virsh` to create five 10G volumes, named as follows:
    ```
    sudo virsh vol-create-as DB_Team rocky-9-db1.qcow2 10G --format qcow2
    ```
    ```
    sudo virsh vol-create-as DB_Team rocky-9-db2.qcow2 10G --format qcow2
    ```
    ```
    sudo virsh vol-create-as DB_Team rocky-9-db3.qcow2 10G --format qcow2
    ```
    ```
    sudo virsh vol-create-as DB_Team rocky-9-db4.qcow2 10G --format qcow2
    ```
    ```
    sudo virsh vol-create-as DB_Team rocky-9-db5.qcow2 10G --format qcow2
    ```
2. Let's take a look at our new volumes:
    ```
    sudo virsh vol-list DB_Team
    ```
3. Let's take a look at the files backing them:
    ```
    ls -la /db/dbstorage
    ```
4. Let's take a look at the list of virtual disk devices on our virtual machine:
    ```
    sudo virsh domblkinfo rocky-9-cli-vm --all
    ```
5. Get the next virtual device name.  It should be `/dev/vdb`.

6. Before we make any edits, we will need to shut down the guest:
    ```
    sudo virsh shutdown rocky-9-cli-vm
    ```
7. Now, let's attach these volumes to the virtual machine:
    ```
    sudo virsh attach-disk rocky-9-cli-vm --source /db/dbstorage/rocky-9-db1.qcow2 --target vdb --cache none --driver qemu --subdriver qcow2 --config
    ```
    ```
    sudo virsh attach-disk rocky-9-cli-vm --source /db/dbstorage/rocky-9-db2.qcow2 --target vdc --cache none --driver qemu --subdriver qcow2 --config
    ```
    ```
    sudo virsh attach-disk rocky-9-cli-vm --source /db/dbstorage/rocky-9-db3.qcow2 --target vdd --cache none --driver qemu --subdriver qcow2 --config
    ```
    ```
    sudo virsh attach-disk rocky-9-cli-vm --source /db/dbstorage/rocky-9-db4.qcow2 --target vde --cache none --driver qemu --subdriver qcow2 --config
    ```
    ```
    sudo virsh attach-disk rocky-9-cli-vm --source /db/dbstorage/rocky-9-db5.qcow2 --target vdf --cache none --driver qemu --subdriver qcow2 --config
    ```
8. Let's take a look at the list of virtual disk devices on our virtual machine:
    ```
    sudo virsh domblkinfo rocky-9-cli-vm --all
    ```
9. Start the domain:
    ```
    sudo virsh start rocky-9-cli-vm
    ```
10. Log in to the virtual machine using `virsh console` as 'tom':
    ```
    sudo virsh console rocky-9-cli-vm
    ```
11. Confirm that the devices are available to the operating system:
    ```
    ls -la /dev/vd*
    ```
    ```
    sudo fdisk -l | more
    ```
12. Use **CTRL** + **]** to exit the `virsh` console.

    The DB team hasn't decided exactly how they want the storage configured from here, so we're going to leave it as-is for now.

We now have the additional database storage configured!

#### Managing Snapshots Using `virsh`

The DB team has requested that we install `mariadb` before we continue.  We've done a lot of work at this point, so we'd like to take a snapshot before we install `mariadb`, *just in case*.

1. Let's take a look at the list of snapshots for our virtual machine:
    ```
    sudo virsh snapshot-list rocky-9-cli-vm
    ```
2. Now, take a snapshot of the virtual machine:
    ```
    sudo virsh snapshot-create-as rocky-9-cli-vm pre-mariadb
    ```
3. Let's take a look at the list of snapshots for our virtual machine:
    ```
    sudo virsh snapshot-list rocky-9-cli-vm
    ```
4. Now, let's install `mariadb` on our virtual machine.  Connect to the console again:
    ```
    sudo virsh console rocky-9-cli-vm
    ```
5. Install `mariadb`:
    ```
    sudo dnf -y install mariadb
    ```
6. Let's confirm that `mariadb` is installed:
    ```
    sudo dnf list installed | grep -i mariadb
    ```
Fantastic!  We're done here.  Use **CTRL** + **]** to exit the `virsh` console.

Well, until the DB team sends us an e-mail letting us know that they wanted `postgresql`, not `mariadb`.

Have no fear; we have our snapshot to bail us out!

1. Let's take a look at information about our snapshots:
    ```
    sudo virsh snapshot-info rocky-9-cli-vm --current
    ```
2. Let's roll back our snapshot:
    ```
    sudo virsh snapshot-revert rocky-9-cli-vm --current
    ```
3. Connect to the console again:
    ```
    sudo virsh console rocky-9-cli-vm
    ```
4. Let's confirm that `mariadb` is NOT installed:
    ```
    sudo dnf list installed | grep -i mariadb
    ```
5. Install `postgresql`:
    ```
    sudo dnf -y install postgresql
    ```
6. Let's confirm that `postgresql` is installed:
    ```
    sudo dnf list installed | grep -i postgresql
    ```
7. Fantastic!  We're done here.  Log out.  Use **CTRL** + **]** to exit the `virsh` console.

That snapshot really saved us!  We're even closer now to having our database server template!