The oVirt Hosted Engine is the "brain" of the oVirt infrastructure.  While it is robust and reliable, sometimes you will need to troubleshoot or manage it outside of the web interface.  We will take a look at the `hosted-engine` command, an important tool in managing an oVirt installation.  We will also take a look at backing up the hosted engine using the `engine-backup` command.

#### Reference Links

[Backups and Migration \| oVirt](https://www.ovirt.org/documentation/administration_guide/index.html#chap-Backups_and_Migration)
[Dashboard \| oVirt](https://www.ovirt.org/documentation/administration_guide/index.html#chap-System_Dashboard)
[Chapter 3. Troubleshooting a Self-Hosted Engine Deployment Red Hat Virtualization 4.2 \| Red Hat Customer Portal](https://access.redhat.com/documentation/en-us/red_hat_virtualization/4.2/html/self-hosted_engine_guide/troubleshooting)

### Managing the oVirt Hosted Engine

oVirt comes with a rich, web-based management interface, but how do we manage the hosted engine that provides this interface?

#### Using the `hosted-engine` Command

In order to use the `hosted-engine` command, we need to log in to the host where our hosted engine is running.  You can use SSH.

The `hosted-engine` command is used to manage the oVirt self-hosted engine.

In order to get HELP, use:
```
sudo hosted-engine --help
```
This will show all the commands and their usage.

Quite often, we will want to confirm the hosted engine VM's status.  We can do that with:
```
sudo hosted-engine --vm-status
```
This will show us the current status of the hosted engine.

We can shut down, power off (force), or start the hosted engine with the `hosted-engine` command.  This comes in handy if we have an unresponsive VM. We will take a look at this when we patch the hosted engine.

#### Patching the Hosted Engine

We need to do a little maintenance on the hosted engine before we continue deploying virtual machines for our WMS team.  We're going to check for patches and install them if they are available.

In order to perform important maintenance, we need to put the hosted engine into **'global' maintenance mode**:

1. Log out of the web interface, then run:
    ```
    sudo hosted-engine --set-maintenance --mode=global
    ```
2. Check the status of the hosted engine to confirm we are in maintenance mode:
    ```
    sudo hosted-engine --vm-status
    ```
3. Let's attach to the hosted engine's console:
    ```
    sudo hosted-engine --console
    ```
4. Hit **Enter**, then log in as `root` using the password you created when you installed oVirt.

5. Once you're logged in as `root`, use `dnf` to install operating system updates:
    ```
    dnf -y update
    ```
    On the Engine machine, check if updated packages are available:
    ```
    engine-upgrade-check
    ```

6. When updates are installed, log out and disconnect from the console using `CTRL`-`]`.

7. Let's shut down the hosted engine:
    ```
    sudo hosted-engine --vm-shutdown
    ```
8. Check the status of the hosted engine:
    ```
    sudo hosted-engine --vm-status
    ```
9. Once the hosted engine is shut down, start it back up:
    ```
    sudo hosted-engine --vm-start
    ```
10. Check the status of the hosted engine:
    ```
    sudo hosted-engine --vm-status
    ```
11. Connect to the console:
    ```
    sudo hosted-engine --console
    ```
12. Log in as `root`.

#### Backing up the hosted engine using `engine-backup`

In case of disaster, we need to create periodic backups of our hosted engine:

1. Create a full backup:
    ```
    engine-backup --scope=all --mode=backup --file=/root/full_he_backup.tar --log=/root/full_he_backup.log
    ```
2. Take a look at the backup files:
    ```
    ls -al /root/*backup*
    ```
3. Create an engine database backup:
    ```
    engine-backup --scope=files --scope=db --mode=backup --file=/root/db_he_backup.tar --log=/root/db_he_backup.log
    ```
4. Take a look at the backup files:
    ```
    ls -al /root/*backup*
    ```
5. Log out and disconnect from the console using **CTRL** + **]**.
6. Take the hosted engine out of *global maintenance mode*:
    ```
    sudo hosted-engine --set-maintenance --mode=none
    ```
7. Check the status of the hosted engine:
    ```
    sudo hosted-engine --vm-status
    ```
We're good to go now.

#### Moving the hosted engine to another host

Connect to oVirt Engine Web Interface:

1. In a web browser, navigate to `https://manager-fqdn`, replacing `manager-fqdn` with the FQDN for your oVirt hosted engine.  If you're not already logged in, log in with the `admin` user and the password you set when you configured the hosted engine.  Enter the **Administration Portal**

2. Navigate to *Compute*, then *Virtual Machines*. Our environment isn't configured to migrate VMs between hosts, but you can take a look at how it's done.

3. Right-click on the `HostedEngine` VM, and select **Edit**.  Click on **Host** and check out the options for where the hosted engine can run.  Click on **Cancel**.

4. Right-click on the `HostedEngine` VM, and select **Migrate**.  Check out the options in the *Migrate VMs* wizard.  Click on **Cancel**.

5. You can also highlight the `HostedEngine` VM, then click on the **Migrate** button in the upper right-hand corner.  This will put you in the *Migrate VMs* wizard, as above.