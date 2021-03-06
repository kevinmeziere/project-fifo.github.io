---
layout: default
category: howl
section: howl/administration
title: Howl Administration
---
# Howl Administration
The Howl admin command is `/opt/local/fifo-howl/bin/howl-admin`.

## General managemnet
Howl uses the SMF to manage it's running state so it is restarted in the case of crashes and booted accordingly on system start. howl can be enabled, disabled and restaerted via: `svcadm enable howl`, `svcadm disable howl` and `svcadm restart howl`

### Updating
Howl can be updated by three simple steps.

#### Installing the new package

```bash
pkgin -fy update
pkgin -fy install fifo-howl
```

#### Updating the config
After the newest package is installed the config file should be checked for changes and eddited if needed. The `.example` file will always contain the newest version of the config `diff` is a handy tool to see if some settings need to be added to the existing file.

```bash
diff /opt/loca/fifo-howl/etc/howl.conf /opt/loca/fifo-howl/etc/howl.conf.example
vi /opt/loca/fifo-howl/etc/howl.conf
```

#### Restarting the service
After the config is updated the service needs to be restarted, howl is running clustered and has more then `N` it is often possible to do a rolling update by restarting one by one.

## Cluster management<a id="cluster"></a>

### howl-admin join `<nodename>@<ip>`
Joins a howl cluster, please note that all data on the joining (not the joined) node is deleted.

### howl-admin leave
Cleanly removes a node from the ring. This is helpful when nodes get moved and the cluster downsized

### howl-admin remove `<nodename>@<ip>`
Forcefully removes a node from the ring. This can be used after fatal node cluster.

### howl-admin member-status
Lists the status of each node and the distribution of data over the ring nodes.

```
================================= Membership ==================================
Status     Ring    Pending    Node
-------------------------------------------------------------------------------
valid     100.0%      --      'howl@172.16.0.254'
-------------------------------------------------------------------------------
Valid:1 / Leaving:0 / Exiting:0 / Joining:0 / Down:0
```

### howl-admin ring-status
Gives a extended report ont he ring, including handoffs and downed nodes.

```
================================== Claimant ===================================
Claimant:  'howl@172.16.0.254'
Status:     up
Ring Ready: true

============================== Ownership Handoff ==============================
No pending changes.

============================== Unreachable Nodes ==============================
All nodes are up and reachable
```

### howl-admin status
A simple command that returns the overall cluster status, it returns a propper return code and is useful for scripted rolling updates.


### howl-admin aae-status

Gives a detailed status on the AAE status of the system.

## Log Files
FiFo uses extensive logging to make debugging issue and understanding behaviour. The log files are located in `/var/log/howl/`. There are multiple log files with various severities.


### debug.log
By default the debug log is disabled, it is very verbose and should not be enabled in production systems to enable it uncomment the followig line in the `howl.conf`

```
log.debug.file = /var/log/howl/debug.log
```

### console.log
This file contains logs of the level info and above, usually all interesting logs can be found here.

### error.log
This files contains errors, it usually should be mostly empty but please keep in mind that failing is not a uncommon practice to deal with unexpected behavuiour so sporadic entries might just be fine.
