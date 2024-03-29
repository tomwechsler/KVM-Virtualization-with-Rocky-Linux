Now that we've stood up our instance of oVirt, let's take a look at the management interface.  We'll cover basic navigation and will authenticate and examine some commonly used items.

#### Reference Links

[Administration Guide \| oVirt](https://www.ovirt.org/documentation/administration_guide/index.html)

### Exploring the oVirt Management Interface

#### Connect to the oVirt Engine Web Interface

In a web browser, navigate to `https://manager-fqdn`, replacing `manager-fqdn` with the FQDN for your oVirt hosted engine.  

### The Main Page

**When you first connect to the web interface, you will be presented with a number of resources.**

- Portals:
  - Administration Portal
  - VM Portal
  - Monitoring Portal
- Downloads:
  - Console Client Resources
  - CA Certificate

1. If you're not already logged in, log in with the `admin` user and the password you set when you configured the hosted engine.  
2. Click on the **User** icon in the upper-right hand corner and select **login**.

### VM Portal

The VM Portal provides a streamlined interface for monitoring and managing virtual machines.  We're going to go a little deeper into the VM Portal when we create virtual machines.

### The Administration Portal

Let's return to the main page, and enter the Administration Portal.  This is where we will do most of our work.

#### The oVirt Dashboard

**Header Bar**

- Bookmarks
- Tags
- Tasks
- Events and Alerts Notification
- Guide/About
- User

**Dashboard**

- Refresh button
- Summary of resource utilization and other metrics

#### The Main Navigation Menu

The *Main Navigation Menu* allows you to view and configure various resources in oVirt.  You can minimize the menu by clicking on the icon (with the three lines) in the upper left-hand corner.

**Commonly used items:**

- Compute --> Virtual Machines
- Compute --> Hosts
- Storage --> Domains
- Storage --> Volumes
- Administration --> User Sessions
- Administration --> Users
- Events

Now that you know your way around the oVirt Management Interface, let's perform a few configuration tasks.