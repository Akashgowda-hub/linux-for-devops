# Disk Management

Disk management involves monitoring storage devices, managing partitions, creating filesystems, checking integrity, and mounting storage. Proper disk management is critical for system reliability and performance.

---

# Block Devices Overview

## lsblk - List Block Devices

Show device hierarchy with partition information.

```bash
lsblk
```

**Expected output:**
```
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  100G  0 disk 
├─sda1   8:1    0    1G  0 part /boot
├─sda2   8:2    0   49G  0 part /
└─sda3   8:3    0   50G  0 part /home
sdb      8:16   0  500G  0 disk 
└─sdb1   8:17   0  500G  0 part /data
sr0     11:0    1 1024M  0 rom
```

**Show more details (including size in GB):**
```bash
lsblk -h
```

**Show all devices (including unmounted):**
```bash
lsblk -a
```

---

## fdisk - Partition Listing

List partition information and table.

```bash
sudo fdisk -l
```

**Expected output:**
```
Disk /dev/sda: 100 GiB, 107374182400 bytes, 209715200 sectors
Disk model: QEMU HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x12345678

Device     Boot    Start      End  Sectors Size Id Type
/dev/sda1  *        2048  2099199  2097152   1G 83 Linux
/dev/sda2      2099200 104857599 102758400  49G 83 Linux
/dev/sda3    104857600 209715199 104857600  50G 83 Linux
```

**Show specific disk:**
```bash
sudo fdisk -l /dev/sda
```

---

## parted - Disk Partitioning Tool

Modern alternative to fdisk with GPT support.

```bash
sudo parted -l
```

**Expected output:**
```
Model: QEMU HARDDISK (scsi)
Disk /dev/sda: 100GB
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Disk Flags: 

Number  Start   End    Size   Type     File system Flags
 1      1049kB  1075MB 1074MB primary  ext4        boot
 2      1075MB  54813MB 53738MB primary ext4
 3      54813MB 108749MB 53936MB primary ext4
```

---

# Filesystem Information

## df - Disk Space Usage

Show filesystem disk space usage.

```bash
df -h
```

**Expected output:**
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2        49G   25G   21G  54% /
/dev/sda1      1014M  256M  703M  27% /boot
/dev/sda3        50G   5.2G   42G  12% /home
tmpfs           3.9G     0  3.9G   0% /dev/shm
/dev/sdb1       500G  250G  235G  52% /data
```

**Show inode usage:**
```bash
df -i
```

**Expected output:**
```
Filesystem      Inodes  IUsed   IFree IUse% Mounted on
/dev/sda2      3145728 524287 2621441  17% /
/dev/sda3      6291456 123456 6167999   2% /home
```

---

## du - Directory Space Usage

Show size of directories.

```bash
du -sh /var/log
```

**Expected output:**
```
2.3G    /var/log
```

**Show all directories with sizes:**
```bash
du -sh /* | sort -h
```

**Expected output:**
```
0       /proc
1.2M    /boot
1.5G    /var
2.3G    /home
5.2G    /usr
23G     /
```

---

# Partition Management with fdisk

## Interactive Partition Creation

**Start interactive fdisk:**
```bash
sudo fdisk /dev/sdb
```

**Interactive commands:**
- `m` - Show menu
- `n` - Create new partition
- `d` - Delete partition
- `p` - Print partition table
- `t` - Change type
- `w` - Write and exit
- `q` - Quit without saving

**Example: Create a new partition**
```
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-1048575999, default 2048): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-1048575999, default 1048575999): +100G

Created a new partition 1 of type 'Linux' and of size 100 GiB.

Command (m for help): w
The partition table has been altered.
```

---

## Partition Management with parted

**Non-interactive partition creation:**
```bash
sudo parted /dev/sdb mklabel gpt
sudo parted /dev/sdb mkpart primary 0 100GB
```

**List partitions:**
```bash
sudo parted -l /dev/sdb
```

**Resize partition:**
```bash
sudo parted /dev/sdb resizepart 1 150GB
```

---

# Filesystem Creation

## mkfs - Create Filesystem

**Create ext4 filesystem:**
```bash
sudo mkfs.ext4 /dev/sdb1
```

**Expected output:**
```
mke2fs 1.46.2 (28-Feb-2021)
Creating filesystem with 26214400 4k blocks and 6553600 inodes
Filesystem UUID: 12345678-1234-1234-1234-123456789abc
Superblock backups stored on blocks: 
  32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 4096000, 7962624, 11239424, 20480000, 23887872

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (65536 blocks): done
Writing superblocks and journal agblocks: done

Filesystem successfully created
```

**Create ext4 with custom options:**
```bash
sudo mkfs.ext4 -L data_drive -m 1 /dev/sdb1
```

- `-L` - Set filesystem label
- `-m` - Reserved block percentage (default 5%)

**Create XFS filesystem:**
```bash
sudo mkfs.xfs /dev/sdb1
```

**Create filesystem with specific blocksize:**
```bash
sudo mkfs.ext4 -b 4096 /dev/sdb1
```

---

# Mounting and Unmounting

## mount - Mount Filesystem

**Mount a partition:**
```bash
sudo mount /dev/sdb1 /mnt
```

**Mount with specific options:**
```bash
sudo mount -t ext4 -o defaults,rw,relatime /dev/sdb1 /mnt
```

**Mount options:**
- `defaults` - rw, suid, dev, exec, auto, nouser, async
- `ro` - Read-only
- `rw` - Read-write
- `relatime` - Update access time (performance improvement)
- `noatime` - Don't update access time
- `nosuid` - No setuid/setgid bits

**Verify mount:**
```bash
mount | grep sdb1
```

**Expected output:**
```
/dev/sdb1 on /mnt type ext4 (rw,relatime)
```

---

## umount - Unmount Filesystem

**Unmount partition:**
```bash
sudo umount /mnt
```

**Force unmount (use with caution):**
```bash
sudo umount -f /mnt
```

**Unmount all by device:**
```bash
sudo umount /dev/sdb1
```

**Check if filesystem is in use:**
```bash
lsof /mnt
```

---

## Persistent Mounts with fstab

Edit `/etc/fstab` to mount filesystems at boot.

```bash
sudo nano /etc/fstab
```

**Add this line:**
```
/dev/sdb1  /data  ext4  defaults,relatime  0  2
```

**Format:**
- Device: `/dev/sdb1`
- Mount point: `/data`
- Filesystem type: `ext4`
- Mount options: `defaults,relatime`
- Dump (0=never, 1=every reboot)
- Pass (0=no check, 1=root, 2=other)

**Mount all filesystems from fstab:**
```bash
sudo mount -a
```

**Verify fstab syntax:**
```bash
sudo mount -a --no-mtab --fake
```

---

# Filesystem Integrity Checking

## fsck - Filesystem Check

**Check filesystem (read-only):**
```bash
sudo fsck -n /dev/sdb1
```

**Expected output:**
```
fsck from util-linux 2.36.1
e2fsck 1.46.2 (28-Feb-2021)
/dev/sdb1: clean, 12345/6553600 files, 1234567/26214400 blocks
```

**Repair filesystem:**
```bash
sudo fsck -y /dev/sdb1
```

- `-n` - Read-only check
- `-y` - Assume yes to all repairs
- `-f` - Force check even if clean

**Important:** Only run fsck on unmounted filesystems!

```bash
# If partition is mounted, schedule check at next boot
sudo touch /forcefsck
sudo shutdown -r now  # Reboot
```

---

## e2fsck - Extended Filesystem Check

For ext2/3/4 filesystems.

```bash
# Check only
sudo e2fsck -n /dev/sdb1

# Check and repair
sudo e2fsck -y /dev/sdb1

# Aggressive repair
sudo e2fsck -p /dev/sdb1
```

---

## xfs_repair - XFS Filesystem Repair

For XFS filesystems.

```bash
# Analyze only
sudo xfs_repair -n /dev/sdb1

# Repair
sudo xfs_repair /dev/sdb1
```

---

# Filesystem Information

## blkid - Block Device IDs

Show UUID and filesystem information.

```bash
blkid
```

**Expected output:**
```
/dev/sda1: UUID="a1b2c3d4-e5f6-7890-abcd-ef1234567890" TYPE="ext4" PARTUUID="12345678-01"
/dev/sda2: UUID="f1e2d3c4-b5a6-9087-efcd-ba0987654321" TYPE="ext4" PARTUUID="12345678-02"
/dev/sdb1: UUID="9f8e7d6c-5b4a-3210-fe dcba-9876543210" TYPE="xfs" PARTUUID="87654321-01"
```

**Show specific device:**
```bash
blkid /dev/sda1
```

---

## tune2fs - Adjust ext4 Parameters

Check ext4 filesystem parameters:

```bash
sudo tune2fs -l /dev/sdb1 | head -30
```

**Expected output:**
```
Filesystem UUID:          12345678-1234-1234-1234-123456789abc
Filesystem features:      has_journal ext_attr resize_inode dir_index filetype needs_recovery
Inode count:              6553600
Block count:              26214400
Filesystem created:       Tue Jan 16 10:30:45 2024
Last mounted on:          /data
Last write time:          Tue Jan 16 10:35:20 2024
Mount count:              5
Maximum mount count:      -1
```

**Change filesystem label:**
```bash
sudo tune2fs -L new_label /dev/sdb1
```

---

# Disk Usage Analysis

## Find Large Files

**Find files larger than 100MB:**
```bash
find / -type f -size +100M -exec ls -lh {} \; | head -20
```

**Expected output:**
```
-rw-r--r-- 1 root root 512M Jan 15 10:30 /var/lib/mysql/ibdata1
-rw-r--r-- 1 root root 256M Jan 16 09:15 /var/log/debug.log
-rw-r--r-- 1 www-data www-data 150M Jan 16 10:00 /var/www/uploads/video.mp4
```

## Directory Size Summary

**Get sizes of top-level directories:**
```bash
du -h --max-depth=1 / | sort -rh
```

**Expected output:**
```
23G     /
5.2G    /usr
3.1G    /var
2.3G    /home
1.5G    /opt
512M    /boot
```

---

# Before/After Example: Adding New Disk

### Before - Check current state:
```bash
lsblk
# Only has /dev/sda with partitions

df -h
# Limited space, almost full
```

### Steps:
```bash
# 1. Physical disk is added to system
lsblk
# Now shows /dev/sdb

# 2. Create partition
sudo fdisk /dev/sdb  # or use parted

# 3. Create filesystem
sudo mkfs.ext4 -L data /dev/sdb1

# 4. Create mount point
sudo mkdir -p /data

# 5. Mount temporary
sudo mount /dev/sdb1 /data

# 6. Verify
df -h
lsblk

# 7. Make permanent in fstab
sudo nano /etc/fstab
# Add: /dev/sdb1  /data  ext4  defaults,relatime  0  2

# 8. Test fstab
sudo mount -a
```

### After - New disk is usable:
```bash
df -h
# Shows /dev/sdb1 mounted at /data with full capacity available
```

---

# Best Practices

1. **Always backup before fsck** - Filesystem checks can fail
2. **Don't fsck mounted filesystems** - Unmount or use read-only
3. **Use labels and UUIDs** - More reliable than device names
4. **Monitor disk usage** - Alert when reaching 80% capacity
5. **Separate mount points** - /home, /var, /opt on different partitions
6. **Use appropriate filesystems** - ext4 (default), xfs (performance), btrfs (advanced)
7. **Enable journaling** - Protect against crash-induced corruption
8. **Regular backups** - Disk corruption is inevitable

---

# Summary

Disk management involves understanding device structure, creating and managing partitions, creating filesystems, mounting storage, and maintaining filesystem integrity. Proper disk management ensures reliability, performance, and efficient use of storage resources.