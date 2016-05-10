---
layout: post
type: post
tags: 
  - Linux
  - Centos 7
  - Networking
published: true
title: Network Bonding Centos 7
---
Network bonding is a method of combining (joining) two or more network interfaces together into a single interface. It will increase the network throughput, bandwidth and will give redundancy. If one interface is down or unplugged, the other one will keep the network traffic up and alive. Network bonding can be used in situations wherever you need redundancy, fault tolerance or load balancing networks.
There are different bonding modes described below:
    
    mode=0 (balance-rr)

    Round-robin policy: It the default mode. It transmits packets in sequential order from the first available slave through the last. This mode provides load balancing and fault tolerance.

    mode=1 (active-backup)

    Active-backup policy: In this mode, only one slave in the bond is active. The other one will become active, only when the active slave fails. The bond’s MAC address is externally visible on only one port (network adapter) to avoid confusing the switch. This mode provides fault tolerance.

    mode=2 (balance-xor)

    XOR policy: Transmit based on [(source MAC address XOR’d with destination MAC address) modulo slave count]. This selects the same slave for each destination MAC address. This mode provides load balancing and fault tolerance.

    mode=3 (broadcast)

    Broadcast policy: transmits everything on all slave interfaces. This mode provides fault tolerance.

    mode=4 (802.3ad)

    IEEE 802.3ad Dynamic link aggregation. Creates aggregation groups that share the same speed and duplex settings. Utilizes all slaves in the active aggregator according to the 802.3ad specification.

    Prerequisites:

    – Ethtool support in the base drivers for retrieving the speed and duplex of each slave.
    – A switch that supports IEEE 802.3ad Dynamic link aggregation. Most switches will require some type of configuration to enable 802.3ad mode.

    mode=5 (balance-tlb)

    Adaptive transmit load balancing: channel bonding that does not require any special switch support. The outgoing traffic is distributed according to the current load (computed relative to the speed) on each slave. Incoming traffic is received by the current slave. If the receiving slave fails, another slave takes over the MAC address of the failed receiving slave.

    Prerequisite:

    – Ethtool support in the base drivers for retrieving the speed of each slave.

    mode=6 (balance-alb)

Adaptive load balancing: includes balance-tlb plus receive load balancing (rlb) for IPV4 traffic, and does not require any special switch support. The receive load balancing is achieved by ARP negotiation. The bonding driver intercepts the ARP Replies sent by the local system on their way out and overwrites the source hardware address with the unique hardware address of one of the slaves in the bond such that different peers use different hardware addresses for the server.

  I have a Dell PowerEdge 2950 with Centos 7 installed, It has 2 NICs as shown below:
1.	enp0s8.
2.	enp0s9.
We’ll combine enp0s8 with enp0s9 to make up bond0.
In Centos 7 you have to load the bonding module(this is a special kernel module that provides the linux bonding driver) since it is not loaded by default.
    modprobe --first-time bonding
You can then run: modinfo bonding to view the module information.
Now we’ll create a bond0 config file, this will be done in /etc/sysconfig/network-scripts directory.
Run the following command as root user to create the bond0 file.
    vi /etc/sysconfig/network-scripts/ifcfg-bond0 
add the following lines to that file(the above command creates the ifcfg-bond0 file since it doesn’t exist yet)

    DEVICE=bond0
    NAME=bond0
    TYPE=Bond
    BONDING_MASTER=yes
    IPADDR=192.168.0.69
    PREFIX=24
    ONBOOT=yes
    BOOTPROTO=none
    BONDING_OPTS="mode=1 miimon=100

Note: The BONDING_OPTS describes the bonding mode which in our case will be active-backup. Save and close the file.
Next we will modify the enp0s8 and enp0s9 config files

    Edit file /etc/sysconfig/network-scripts/ifcfg-enp0s8,

    vi /etc/sysconfig/network-scripts/ifcfg-enp0s8
Modify the file as shown below.

    HWADDR="08:00:27:04:03:86"
    TYPE="Ethernet"
    BOOTPROTO="none"
    DEFROUTE="yes"
    PEERDNS="yes"
    PEERROUTES="yes"
    IPV4_FAILURE_FATAL="no"
    IPV6INIT="yes"
    IPV6_AUTOCONF="yes"
    IPV6_DEFROUTE="yes"
    IPV6_PEERDNS="yes"
    IPV6_PEERROUTES="yes"
    IPV6_FAILURE_FATAL="no"
    NAME="enp0s8"
    UUID="a97b23f2-fa87-49de-ac9b-39661ba9c20f"
    ONBOOT="yes"
    MASTER=bond0
    SLAVE=yes
Then, Edit file /etc/sysconfig/network-scripts/ifcfg-enp0s9,

    vi /etc/sysconfig/network-scripts/ifcfg-enp0s9
Modify the file as shown below.

    HWADDR=08:00:27:E7:ED:8E
    TYPE=Ethernet
    BOOTPROTO=none
    DEFROUTE=yes
    PEERDNS=yes
    PEERROUTES=yes
    IPV4_FAILURE_FATAL=no
    IPV6INIT=yes
    IPV6_AUTOCONF=yes
    IPV6_DEFROUTE=yes
    IPV6_PEERDNS=yes
    IPV6_PEERROUTES=yes
    IPV6_FAILURE_FATAL=no
    NAME=enp0s9
    UUID=e2352c46-e1f9-41d2-98f5-af24b127b3e7
    ONBOOT=yes
    MASTER=bond0
    SLAVE=yes
    Save and close the files.

Now, activate the Network interfaces.

    ifup ifcfg-enp0s8
    ifup ifcfg-enp0s9
Now, enter the following command to make Network Manager aware the changes.

    nmcli con reload
Restart network service to take effect the changes.

    systemctl restart network
Test Network Bonding

Now enter the following command to check whether the bonding interface bond0 is up and running:

    cat /proc/net/bonding/bond0
Sample output:

    Ethernet Channel Bonding Driver: v3.7.1 (April 27, 2011)

    Bonding Mode: fault-tolerance (active-backup)
    Primary Slave: None
    Currently Active Slave: enp0s8
    MII Status: up
    MII Polling Interval (ms): 100
    Up Delay (ms): 0
    Down Delay (ms): 0

    Slave Interface: enp0s8
    MII Status: up
    Speed: 1000 Mbps
    Duplex: full
    Link Failure Count: 0
    Permanent HW addr: 08:00:27:5d:ad:75
    Slave queue ID: 0

    Slave Interface: enp0s9
    MII Status: up
    Speed: 1000 Mbps
    Duplex: full
    Link Failure Count: 0
    Permanent HW addr: 08:00:27:48:93:cd
    Slave queue ID: 0
As you see in the above output, the bond0 interface is up and running and it is configured as active-backup(mode1) mode. In this mode, only one slave in the bond is active. The other one will become active, only when the active slave fails.
To view the list of network interfaces and their IP address, enter the following command:

    ip addr

That’s it.

Configure multiple IP addresses for bond0

I want to assign multiple IP addresses to bond0 interface. What should i do? Very simple, just create an alias for the bond0 interface and assign multiple IP addresses.

For example we want to assign IP address 192.168.0.151 to bond0. To create an alias for bond0, copy the existing configuration file(ifcfg-bond0) to a new configuration file(ifcfg-bond0:1).

    cp /etc/sysconfig/network-scripts/ifcfg-bond0 /etc/sysconfig/network-scripts/ifcfg-bond0:1
Then edit the alias file /etc/sysconfig/network-scripts/ifcfg-bond0:1,

    vi /etc/sysconfig/network-scripts/ifcfg-bond0:1
Modify the device name and IP address as shown below.

    DEVICE=bond0:1
    NAME=bond0
    TYPE=Bond
    BONDING_MASTER=yes
    IPADDR=192.168.0.151
    PREFIX=24
    ONBOOT=yes
    BOOTPROTO=none
    BONDING_OPTS="mode=1 miimon=100"

Here,

    bond0:1 – Device name
    192.168.0.151 – IP address of bond0:1
Save and close the file. Restart network service to take effect the saved changes.

    systemctl restart network
Now list out the network interfaces and their IP address using the command:

    ip addr

The alias bond0:1 has been created and it’s up now.
