How is to clone a disk on macOS?  
This manual avoids use of modern packages like *coreutils* for backward compatibility reasons: `dd` is an old Unix command.

Obtain a list of disks attached:
```bash
Vitals-Mac-mini:~ vital$ diskutil list
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *251.0 GB   disk0
   1:                        EFI EFI                     314.6 MB   disk0s1
   2:                 Apple_APFS Container disk1         250.7 GB   disk0s2

/dev/disk1 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme -                      +250.7 GB   disk1
                                 Physical Store disk0s2
   1:                APFS Volume Macintosh HD - Data     104.2 GB   disk1s1
   2:                APFS Volume Preboot                 82.1 MB    disk1s2
   3:                APFS Volume Recovery                528.5 MB   disk1s3
   4:                APFS Volume VM                      2.1 GB     disk1s4
   5:                APFS Volume Macintosh HD            11.2 GB    disk1s5

/dev/disk2 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *120.0 GB   disk2
   1:                        EFI NO NAME                 536.9 MB   disk2s1
   2:           Linux Filesystem                         119.5 GB   disk2s2

/dev/disk3 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *275.1 GB   disk3
   1:                        EFI EFI                     209.7 MB   disk3s1
   2:                  Apple_HFS Crucial_CT275MX300SSD1  274.7 GB   disk3s2

```
In this example a smaller Linux source disk /dev/disk2 has to be copied on a bigger target disk /dev/disk3   
Keep in mind that MacOS as BSD family uses /dev/rdisk as a raw disk what is something closer to a physical disk.

Unmount a target disk
```bash
diskutil unmountDisk /dev/rdisk3
```

Replicate a source disk on a target disk:  
<small>`1m` is for macOS what means copy by blocks of 1 Megabyte size (but `1M` when `dd` runs in Linux OS)</small>
```bash
sudo dd if=/dev/rdisk2 of=/dev/rdisk3 bs=1m
```

As a matter of fact `if` and `of` parameters for `dd` command may be disks or image files:  
<small>
`conv=notrunc` option for `dd` is only important to prevent truncation when writing into a file. This has no effect on a block device.</small>
```bash
sudo dd if=/dev/rdisk2 of=~/Downloads/my-linux.iso bs=1m conv=notrunc
```
