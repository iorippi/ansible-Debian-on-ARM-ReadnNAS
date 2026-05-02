# NETGEAR RN102
# Maintenance Instructions

When adding disk

When swapping one disk out of RAID1, make sure the new disk is
the same or the bigger in size.

```
# Clone partition table (easiest)
$ sfdisk -d /dev/sda | sfdisk /dev/sdb
```
