The Virtual Machine Manager (virt-manager) is a desktop virtual machine manager that gives users a graphical user interface to manage virtual machines.  In this lesson, we will walk through some common tasks we might want to perform on a virtual machine, using Virtual Machine Manager.

### Managing a Virtual Machine Using 'virt-manager'

#### Changing the system state

1. Now that we have a fresh installation of Rocky 9 (minimal), let's patch it before we use it.
    1. Start the virtual machine:
      - Log in as 'tom'
    2. Patch the virtual machine.
    ```
    sudo dnf -y update
    ```
    3. Shut the virtual machine down.  Use the *Virtual Machine* --> *Shut Down* --> *Shut Down* option.

#### Clone VM

Our Web Developer has requested a new server for development on a new project.  Let's clone a new server for them.

1. In the GUI, right-click on the `rocky-9-vm` VM and select **Clone**.  
2. Set the *Name* to 'rocky-9-webserver' and leave the rest as defaults.

#### VM Snapshot

Bring the clone up.  Our Web Developer asked us for the NGINX web server on this cloned server.  We're going to install NGINX.

Change the hostname of the clonend Server to rocky-9-webserver. To do so, run the following command:
```
sudo hostnamectl set-hostname rocky-9-webserver
```

Next, though, let's take a snapshot of this cloned server, just in case.

1. Click on *Manage VM Snapshots*, then in the lower left-hand corner, click on the **+** button.

2. Give the snapshot the name of `Safety_Net` and leave the rest as defaults.  We have a point in time snapshot now, just in case.  Go back to the console.

3. Confirm that NGINX **IS NOT** installed:

    ```
    dnf list installed | grep -i nginx
    ```

4. Install NGINX:

    ```
    sudo dnf -y install nginx
    ```

5. Confirm that NGINX **IS** installed:

    ```
    dnf list installed | grep -i nginx
    ```

Fantastic!  We're good! At least, until after lunch, when we get an e-mail from our Web Developer, asking for *Apache Web Server*, instead.

Not a problem, we took a snapshot.  

#### Roll back.

1. Click on **Manage VM Snapshots**, then in the lower left-hand corner, click on the **play** button.  Go back to the console.

2. Confirm that NGINX **IS NOT** installed:

```
dnf list installed | grep -i nginx
```

3. Now we're good to install Apache Web Server.
```
sudo dnf -y install httpd
```
```
sudo systemctl enable httpd
```
```
sudo systemctl start httpd
```
```
sudo systemctl status httpd
```
```
curl http://127.0.0.1
```
Now we have a fresh and functional installation of Apache!

#### Modify Virtual Hardware

As part of this deployment of the new web server, we have been asked to increase the RAM from 4GB to 8GB.

1. Shut down the server, then click on the button that looks like a light bulb, the 'Show virtual hardware details' button), and select 'Memory'.  Adjust this from 4096 to 8192.  Bring the server back up.

2. When the server is up, log in as 'tom' and check the memory:
    ```
    more /proc/meminfo
    ```
    We should see 8GB RAM.

We now have a happy Web Developer!  Almost.