---
title: "Preparing a Portable Disk for macOS"
date: "2018-02-16"
categories: 
  - "system-administration"
tags: 
  - "administration"
  - "macos"
---

I purchased a Western Digital external hard disk from Best Buy. On the shelf they had one for "Windows" and one for "MacOS" and the MacOS-compatible one was priced $20 higher than the Windows one. Marketing will not fool me today - time to reformat NTFS to a JHFS+ filesystem.

Immediately I plugged the disk into my MacBook Pro and opened Disk Utility. After erasing the NTFS partition, I attempted to create a new JHFS+ partition only to be met with the output "Erase process has failed, press done to continue." Expanding the output displayed the error "Mediakit reports not enough space on device for requested operation." Confused as I had removed all the existing partitions, I attempted to manually create the new partition as Macintosh Extended (Journaled). No matter the sizing or naming, partition creation would always fail.

After hunting around online, I finally reached a workable solution. I am working on MacOS High Sierra which enabled the newest APFS file system. If you wish to format your external with APFS, you will first need to format it as HFS+, then subsequently migrate it to APFS.

First you need to get the name of the disk you are trying to format. On my MacBook with High Sierra there were 2 existing system disks "disk0" for the recovery files and APFS container volume and "disk1" which is a synthesized set of APFS volumes within the container. As such, the external hard disk appeared as "disk2", but this may vary on your system depending on what you have mounted.

```
diskutil list
```

Once you've identified your disk, unmount it.

```
diskutil unmountDisk force disk2
```

Once this completes, you will want to overwrite the boot sector for the external.

```
sudo dd if=/dev/zero of=/dev/disk2 bs=1024 count=1024
```

Lastly, you will want to partition the disk as JHFS+, including the name for the new volume.

```
diskutil partitionDisk disk2 GPT JHFS+ "My Passport" 0g
```

The magic here is removing the boot sector (also called the MBR). The MBR of a disk manages both boot information and the partition table. If that table is unreadable or corrupt, it can render partitions unmanageable. By zeroing out the boot sector of the disk it forces MacOS to create a new GUID partition scheme that it can manage.

Here is the output of the entire formatting operation as run on my system.

```

pearce at Deans-MacBook-Pro in ~/Projects
$ diskutil list
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *251.0 GB   disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:                 Apple_APFS Container disk1         250.8 GB   disk0s2

/dev/disk1 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme - +250.8 GB   disk1
                                 Physical Store disk0s2
   1:                APFS Volume Macintosh HD            116.0 GB   disk1s1
   2:                APFS Volume Preboot                 19.8 MB    disk1s2
   3:                APFS Volume Recovery                509.8 MB   disk1s3
   4:                APFS Volume VM                      2.1 GB     disk1s4

/dev/disk2 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *1.0 TB     disk2
   1:                  Apple_HFS                         1.0 TB     disk2s1


pearce at Deans-MacBook-Pro in ~/Projects
$ diskutil unmountDisk force disk2
Forced unmount of all volumes on disk2 was successful

pearce at Deans-MacBook-Pro in ~/Projects
$ sudo dd if=/dev/zero of=/dev/disk2 bs=1024 count=1024
Password:
1024+0 records in
1024+0 records out
1048576 bytes transferred in 0.491402 secs (2133846 bytes/sec)

pearce at Deans-MacBook-Pro in ~/Projects
$ diskutil partitionDisk disk2 GPT JHFS+ "My Passport" 0g
Started partitioning on disk2
Unmounting disk
Creating the partition map
Waiting for partitions to activate
Formatting disk2s2 as Mac OS Extended (Journaled) with name My Passport
Initialized /dev/rdisk2s2 as a 931 GB case-insensitive HFS Plus volume with 
a 81920k journal
Mounting disk
Finished partitioning on disk2
/dev/disk2 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *1.0 TB     disk2
   1:                        EFI EFI                     209.7 MB   disk2s1
   2:                  Apple_HFS My Passport             999.8 GB   disk2s2
```
