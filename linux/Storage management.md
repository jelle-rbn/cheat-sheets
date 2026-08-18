# Storage Management Cheat Sheet

## Disk Devices

### Device Listing & Identification

| Command                   | Description                                                      |
| :------------------------ | :--------------------------------------------------------------- |
| `lsblk`                   | Display all block devices in a tree structure                    |
| `lsblk --nvme`            | Display NVMe devices only                                        |
| `ls -l /dev/sd*`          | List SCSI/SATA/USB disks and partitions                          |
| `fdisk -l`                | Display all detected disks and partition tables                  |
| `fdisk -l <device>`       | Display detailed partition layout for a specific device          |
| `dmesg \| grep <pattern>` | Search kernel boot logs for disk detection events                |
| `cat /proc/scsi/scsi`     | Raw SCSI subsystem view of detected devices                      |
| `lshw -class disk`        | Display detailed hardware information for all disks              |
| `lshw -class volume`      | Display detailed information for volumes (partitions, LVM, etc.) |
| `lsscsi`                  | List SCSI/SATA/USB devices using SCSI emulation                  |
| `lsscsi -c`               | Compact output of `lsscsi`                                       |

### Disk Wiping & Destruction Tests

| Command                                                  | Description                                                                      |
| :------------------------------------------------------- | :------------------------------------------------------------------------------- |
| `sudo badblocks -ws <device>`                            | Destructive write test over multiple passes (detects bad blocks, **wipes data**) |
| `sudo dd if=/dev/zero of=<device> bs=1M status=progress` | Overwrites the entire device with zeros (single pass)                            |

### Hardware Parameters (ATA/SATA/SCSI)

| Command                   | Description                                       |
| :------------------------ | :------------------------------------------------ |
| `sudo hdparm <device>`    | Basic information and status for ATA/SATA drives  |
| `sudo hdparm -i <device>` | Legacy drive identification parameters            |
| `sudo hdparm -I <device>` | Detailed ATA/SATA capabilities and specifications |
| `sudo sdparm <device>`    | View and modify SCSI/SAS drive parameters         |

### SMART Monitoring

| Command                     | Description                                                   |
| :-------------------------- | :------------------------------------------------------------ |
| `sudo smartctl --scan`      | Scan for all SMART-compatible drives                          |
| `sudo smartctl -a <device>` | Complete SMART report: health status, error logs, and metrics |

---

## Disk Partitioning

### Partition Discovery

| Command                | Description                                          |
| :--------------------- | :--------------------------------------------------- |
| `fdisk -l`             | Display all detected disks and partition tables      |
| `fdisk -l <device>`    | Display details for a specific disk                  |
| `cat /proc/partitions` | Show major/minor numbers and block counts per device |
| `cat /proc/devices`    | Map block and character drivers to major numbers     |

### Interactive Partitioning with fdisk (MBR/GPT)

| Command / Prompt     | Description                                       |
| :------------------- | :------------------------------------------------ |
| `fdisk <device>`     | Launch `fdisk` interactive shell on target device |
| `p` _(inside fdisk)_ | Print current partition table                     |
| `n` _(inside fdisk)_ | Create a new partition                            |
| `d` _(inside fdisk)_ | Delete a partition                                |
| `w` _(inside fdisk)_ | Write partition table changes to disk and exit    |
| `q` _(inside fdisk)_ | Exit without saving changes                       |

### Alternative Partitioning Tools

| Command                                   | Description                                                  |
| :---------------------------------------- | :----------------------------------------------------------- |
| `parted <device>`                         | Launch GNU Parted (supports both GPT and MBR)                |
| `cfdisk <device>`                         | Ncurses-based interactive partitioning tool                  |
| `sfdisk -d <device>`                      | Dump disk partition table format as a scriptable text file   |
| `sfdisk -d <src_dev> \| sfdisk <dst_dev>` | Clone partition table layout from source to destination disk |
| `gparted`                                 | Graphical Partition Editor                                   |

### MBR Management & Backups

| Command                                       | Description                                                       |
| :-------------------------------------------- | :---------------------------------------------------------------- |
| `dd if=<device> of=<filename> bs=512 count=1` | Backup MBR (boot loader + partition table) to a file              |
| `dd if=/dev/zero of=<device> bs=512 count=1`  | Destroy MBR boot loader and partition table                       |
| `partprobe`                                   | Force the kernel to re-read the partition table without rebooting |

### Interactive Partitioning with parted

| Command / Prompt               | Description                          |
| :----------------------------- | :----------------------------------- |
| `parted <device>`              | Open `parted` shell on target device |
| `mklabel msdos`                | Create a new MBR partition table     |
| `mklabel gpt`                  | Create a new GPT partition table     |
| `mkpart primary <start> <end>` | Create a primary partition           |
| `print`                        | Display partition layout             |
| `quit`                         | Exit `parted`                        |
| `help`                         | Show available commands              |
| `help <command>`               | Display help for a specific command  |

---

## File Systems

### File System Discovery & Information

| Command                   | Description                                                   |
| :------------------------ | :------------------------------------------------------------ |
| `man fs`                  | Display manual page for Linux file system conventions         |
| `cat /proc/filesystems`   | List file system types currently supported by the kernel      |
| `cat /etc/filesystems`    | List file system types used for auto-detection during mount   |
| `mount \| grep <pattern>` | Search mounted file systems matching pattern                  |
| `df -h`                   | Display file system disk space usage in human-readable format |

### Creating File Systems (Formatting)

| Command                   | Description                                               |
| :------------------------ | :-------------------------------------------------------- |
| `ls -lS /sbin/mk*`        | List all available formatting binaries (`mkfs.*`)         |
| `mkfs <device>`           | Generic `mkfs` wrapper; detects file system automatically |
| `mkfs.ext2 <device>`      | Format partition with ext2 file system                    |
| `mkfs.ext3 <device>`      | Format partition with ext3 file system                    |
| `mkfs.ext4 <device>`      | Format partition with ext4 file system                    |
| `mke2fs -t ext3 <device>` | Format partition with ext3 using `mke2fs`                 |
| `mke2fs -t ext4 <device>` | Format partition with ext4 using `mke2fs`                 |
| `mkfs.vfat <device>`      | Format partition with FAT12/16/32 file system             |
| `mkswap <device>`         | Initialize a swap area on a partition or device           |
| `swapon <device>`         | Enable swap area for system memory paging                 |

### File System Tuning

| Command                            | Description                                         |
| :--------------------------------- | :-------------------------------------------------- |
| `tune2fs -l <device>`              | Display filesystem parameters for ext2/ext3/ext4    |
| `tune2fs -m <percentage> <device>` | Adjust percentage of reserved blocks for super-user |
| `tune2fs -j <device>`              | Add an ext3 journal to an existing ext2 file system |

### File System Integrity Checking (fsck)

| Command              | Description                                                  |
| :------------------- | :----------------------------------------------------------- |
| `ls /sbin/*fsck*`    | List available file system check utilities                   |
| `fsck <device>`      | Check and repair a file system (must be unmounted)           |
| `fsck -p <device>`   | Automatically repair file system without prompting ("preen") |
| `e2fsck <device>`    | Check ext2/ext3/ext4 file systems                            |
| `e2fsck -p <device>` | Automatically repair ext2/ext3/ext4 file systems             |

---

## Mounting Operations

### Mount Points & Local File Systems

| Command                                        | Description                                      |
| :--------------------------------------------- | :----------------------------------------------- |
| `sudo mkdir -p <mountpoint>`                   | Create a directory to serve as a mount point     |
| `sudo mount -t <fstype> <device> <mountpoint>` | Mount file system with explicit type declaration |
| `sudo mount <device> <mountpoint>`             | Mount file system using auto-detection           |
| `sudo umount <mountpoint>`                     | Unmount a mounted file system by mount point     |
| `sudo umount <device>`                         | Unmount a mounted file system by device path     |

### Inspecting Active Mounts

| Command                    | Description                                                           |
| :------------------------- | :-------------------------------------------------------------------- |
| `mount`                    | Display all currently mounted file systems (legacy)                   |
| `findmnt`                  | Display tree-like layout of mounted file systems                      |
| `findmnt --real`           | Filter out pseudo/virtual file systems (show physical/network mounts) |
| `findmnt --fstab`          | List file systems defined in `/etc/fstab`                             |
| `findmnt --json`           | Export mount hierarchy in JSON format                                 |
| `cat /proc/mounts`         | View kernel-level active mount points                                 |
| `cat /proc/self/mountinfo` | View detailed process-specific mount point metadata                   |

### Storage Usage Tracking

| Command              | Description                                           |
| :------------------- | :---------------------------------------------------- |
| `df -h`              | View disk usage and capacity across all active mounts |
| `du -sh <directory>` | Calculate total space used by a specific directory    |

### Persistent Mounts (/etc/fstab)

| File / Command                                           | Description                                      |
| :------------------------------------------------------- | :----------------------------------------------- |
| `/etc/fstab`                                             | Static file system table configured at boot      |
| `<device> <mountpoint> <fstype> <options> <dump> <pass>` | Standard `/etc/fstab` configuration entry syntax |
| `sudo mount -a`                                          | Mount all file systems listed in `/etc/fstab`    |

### Mount Security Options

| Command / Option                             | Description                                          |
| :------------------------------------------- | :--------------------------------------------------- |
| `sudo mount -o ro <device> <mountpoint>`     | Mount file system as Read-Only                       |
| `sudo mount -o noexec <device> <mountpoint>` | Prevent execution of binaries on mounted file system |
| `sudo mount -o nosuid <device> <mountpoint>` | Ignore SetUID and SetGID permission bits             |
| `sudo mount -o noacl <device> <mountpoint>`  | Disable Access Control List (ACL) evaluation         |

### Remote File Systems (CIFS/SMB & NFS)

| Command / Option                                                    | Description                                                         |
| :------------------------------------------------------------------ | :------------------------------------------------------------------ |
| `sudo mount -t cifs -o user=<user> //<server>/<share> <mountpoint>` | Mount a Windows/Samba network share                                 |
| `sudo mount -t nfs <server>:<directory> <mountpoint>`               | Mount an NFS share via hostname                                     |
| `sudo mount -t nfs <ip>:<directory> <mountpoint>`                   | Mount an NFS share via IP address                                   |
| `soft` _(NFS option)_                                               | Abort operation after defined retries fail                          |
| `hard` _(NFS option)_                                               | Hang process and retry indefinitely until server responds (default) |
| `retrans=<X>` _(NFS option)_                                        | Set number of retransmission attempts before timing out             |
| `proto=tcp` / `proto=udp` _(NFS option)_                            | Force transport protocol                                            |

---

## Universally Unique Identifiers (UUIDs)

### Retrieving UUIDs & File System Metadata

| Command                                 | Description                                                                         |
| :-------------------------------------- | :---------------------------------------------------------------------------------- |
| `lsblk -f`                              | Output block devices alongside file system type, volume label, UUID, and mountpoint |
| `sudo blkid`                            | Display UUID and filesystem metadata for all block devices                          |
| `sudo tune2fs -l <device> \| grep UUID` | Query filesystem UUID on an ext2/3/4 partition                                      |
| `uuidgen`                               | Generate a new random UUID string                                                   |

### Persistent Mounts Using UUIDs

| Syntax / Command                                        | Description                                               |
| :------------------------------------------------------ | :-------------------------------------------------------- |
| `UUID=<uuid-string> <mountpoint> <fstype> defaults 0 2` | `/etc/fstab` entry syntax using stable device identifiers |
| `grep UUID /etc/fstab`                                  | Check existing UUID mounts in fstab configuration         |

### Kernel Boot Configuration (GRUB)

| Configuration Line        | Description                                                          |
| :------------------------ | :------------------------------------------------------------------- |
| `root=UUID=<uuid-string>` | Specifies root filesystem in GRUB configuration for boot reliability |
