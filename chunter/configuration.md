---
layout: default
category: chunter
section: chunter/configuration
title: Chunter Configuration
---
# Chunter Configuration

## Configuration file
Chunter's configuration file is located in `/opt/chunter/etc/chunter.conf` it is automatically generated on the first install and not overwritten on updates. Non the less the newst version of the file is always located in `/opt/chunter/etc/chunter.conf.example`.

In addition Chunter keeps a file `/opt/chunter/etc/hostid` that helps identifying the host thorught udpates, hostname changes, and reinstallations.

The configuration file is documented inline but we'll go over go over some more interesting settings here.

### Genereal

#### `ip`
The IP and Port reported to FiFo, this is normally auto detected ut in some cases needs to be changed to the right IP.

#### `reserved_memory`
The amount of memory reserved on the hypervisor that can not be given out to VM's, this can be very helpful to ensure some memory is left for the global zone and ARC.

### S3
When used together with [LeoFS](https://leofs.org) or other S3 compatible systems Chunter stores backups in the object store.

#### `s3_upload_chunk_size`
Size of the chunks for uploading to the S3 storage.

#### `paralell_uploads`
Initial number of paralel uploads to S3.

#### `max_paralell_uploads`
Maximal number of paralel uploads to S3.

#### `preload`
Number of chunks to prefreth when downloading from S3.

#### `s3_download_chunk_size`
Size of the chunks for downloading fomr the S3 storage.

#### `paralell_downloads`
Initial number of paralel downloads from S3.

#### `max_paralell_downloads`
Maximal number of paralel downloads from S3.

### Intervals
Chunter allows the users to configure various intervals that get a balance between accuracy and load, the defaults are sensible values but can be tuned as needed.

#### `update_services_interval`
The interval services for a zone are checked, the default is 10s changes are noted even when the state is the same at least for the nsq logging.

#### `snapshot_update_interval`
The interval the ZFS snapshots are checked this should not be too fast since it will increase load. The default is every 15 minutes. This also does not change too often so increasing it does not make too much sense.

#### `zonemon_interval`
The interval the zones are checked for their state, this means running `zoneadm list -ip` on the node. This is only required if a change is missed due to some hiccup but it ensures that the state of zones is always up to date.

Generally the more often this happen the less are the chances a state is misrepresented but the higher is the load on the system. This operation is fairly inexpensive.

#### `zpool_interval`
The interval in which the zpool is checked for degrated disks. This is a more expensive opperation so should be executed not too frequently, generaly a interval of 15s means that worst case a degraded pool stays undetected for 15s.

#### `arc_interval`
The interval in which the systems ARC status is checked, this is purely informational and slowly changing a higher interval is usually not a issue.
