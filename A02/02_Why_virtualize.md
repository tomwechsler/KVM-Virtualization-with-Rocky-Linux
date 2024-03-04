#### Reference Links

[5 Benefits of Virtualization](https://www.ibm.com/blog/5-benefits-of-virtualization/)

### Why Virtualize?

There are many reasons to consider virtualizing some or all of your physical infrastructure. Here are some of the more prominent ones.

#### Reduce Your Hardware Costs

*Generally, most traditional physically deployed server footprints average 5-15% utilization. It's inefficient.*

By virtualizing, you can:

- Retire older servers, which are more expensive to operate and cost more to support
- Standardize on a single (or minimal number of) hardware configuration(s)
  - Keep spares on-site for quick repair turnaround
  - Reduces complexity of the hardware layer design
  - Reduces maintenance complexity
  - Use commodity, "bang for the buck" hardware

#### Reduce Your Energy Costs

With the consolidation of servers into a reduced hardware footprint comes a reduction in energy consumption, as well as cooling costs. With fewer physical servers, you will also require less physical networking hardware (IP and storage), less cabling, and less support equipment.

#### Reduce Your Footprint

Fewer physical servers means less rack space. If you're in a hosted facility and are paying by the rack, this can mean big savings. If you own your datacenter, you can use the floor space for other things.

#### Workload Management / Fault Tolerance / Maintenance

In an enterprise-level virtualization solution, you can move virtual machines between hosts with no (or minimal interruption). This facilitates moving guests around to balance workloads across hosts.

In a properly-designed virtual infrastructure, there will be enough virtualization hosts so one or more can be taken offline for maintenance while all the guests can continue to run on the remaining hypervisor(s).

In addition, most enterprise-level virtualization solutions, using shared storage, will support moving guests to another hypervisor in the event of an issue or outage.

The ability to take snapshots of guests prior to patching, application upgrades, or other potentially dangerous processes provides a safety net and a quick mechanism to back out changes that might have been less than successful.

#### Disaster Recovery / Migrations

An enormous benefit of virtualization is the ease with which guests can be moved between hosts. This is not only possible within a single datacenter, but across datacenters and in the cloud. Many enterprise-level solutions offer disaster recovery solutions so you don't have to reinvent the wheel.

This same concept applies when it comes to migrating guests to new hardware.

#### Backups

With the ability to take snapshots of virtual disks comes the ability to back up these snapshots. Since this happens outside of the guest operating system, we can use a system-agnostic backup methodology. We can also leverage advanced backup and recovery options, where available, that were not available or were difficult to implement with traditional hardware-based infrastructures.

#### Standardization / Automation

In addition to moving to a homogeneous hardware platform, virtualization provides a clean, predicable environment for guests. This standardization reduces complexity in the datacenter stack.

A good virtualization platform gives you the tools and automation to ease management.