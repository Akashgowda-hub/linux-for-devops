# Storage & LVM (Logical Volume Manager)

LVM (Logical Volume Manager) provides flexible storage management by abstracting physical storage into logical volumes. This allows resizing, snapshotting, and complex storage configurations impossible with traditional partitions.

---

# LVM Component Hierarchy

LVM uses a three-level hierarchy:

```
Physical Volume (PV)    → Physical disk or partition
        ↓
Volume Group (VG)       → Collection of PVs
        ↓
Logical Volume (LV)     → Virtual partition from VG
        ↓
Filesystem              → ext4, xfs, etc.
```

**Example visualization:**
```
/dev/sda1        /dev/sdb1        /dev/sdc1
    │                │                │
    └────────────────┴────────────────┘
                     │
            Volume Group: vg_data
                     │
        ┌────────────┼────────────┐
        │            │            │
    lv_root      lv_var       lv_home
        │            │            │
      ext4         ext4         ext4
        │            │            │
       /            /var        /home
```

---

# Physical Volumes (PV)

## Create Physical Volume

Prepare a disk or partition for LVM.

```bash
# Create PV from entire disk
sudo pvcreate /dev/sdb

# Create PV from partition
sudo pvcreate /dev/sdb1

# Create multiple PVs
sudo pvcreate /dev/sdb /dev/sdc /dev/sdd
```

**Expected output:**
```
Physical volume "/dev/sdb" successfully created.
```

## Display Physical Volumes

**Show all PVs:**
```bash
sudo pvs
```

**Expected output:**
```
  PV         VG      Fmt  Attr PSize  PFree
  /dev/sda3  vg_main lvm2 a--  50.00g  5.00g
  /dev/sdb1  vg_data lvm2 a--  100.00g 10.00g
  /dev/sdc   vg_data lvm2 a--  200.00g 200.00g
```

**Detailed PV information:**
```bash
sudo pvdisplay
```

**Expected output:**
```
--- Physical volume ---
  PV Name               /dev/sdb1
  VG Name               vg_data
  PV Size               100.00 GiB / not usable 0
  Allocatable           yes
  PE Size               4.00 MiB
  Total PE              25600
  Free PE               2560
  Allocated PE          23040
  PV UUID               aBcDeF12-3456-7890-aBcD-eF1234567890
```

## Remove Physical Volume

**Before removing, migrate LV data:**
```bash
# Move LV data off the PV
sudo pvmove /dev/sdb1

# Remove PV from VG
sudo vgreduce vg_data /dev/sdb1

# Remove PV
sudo pvremove /dev/sdb1
```

---

# Volume Groups (VG)

## Create Volume Group

Combine PVs into a VG.

```bash
# Create VG from single PV
sudo vgcreate vg_data /dev/sdb1

# Create VG from multiple PVs
sudo vgcreate vg_large /dev/sdb1 /dev/sdc /dev/sdd
```

**Expected output:**
```
Volume group "vg_data" successfully created
```

## Display Volume Groups

**Show all VGs:**
```bash
sudo vgs
```

**Expected output:**
```
  VG       #PV #LV #SN Attr   VSize   VFree
  vg_main    3   5   0 wz--n- 150.00g  15.00g
  vg_data    1   3   0 wz--n- 100.00g  10.00g
```

**Detailed VG information:**
```bash
sudo vgdisplay vg_data
```

**Expected output:**
```
--- Volume group ---
  VG Name               vg_data
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  4
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                3
  Open LV               3
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               100.00 GiB
  PE Size               4 MiB
  Total PE              25600
  Alloc PE / Size       23040 / 90.00 GiB
  Free  PE / Size       2560 / 10.00 GiB
  VG UUID               vG1234Ab-5678-90cD-eF12-34567890aBcD
```

## Extend Volume Group

Add PV to existing VG.

```bash
# Add PV to VG
sudo vgextend vg_data /dev/sdc1

# Add multiple PVs
sudo vgextend vg_data /dev/sdc1 /dev/sdd1
```

**Expected output:**
```
Volume group "vg_data" successfully extended
```

## Remove Volume Group

**Before removing, delete all LVs:**
```bash
# List LVs
sudo lvs -o lv_name,vg_name

# Remove LVs (data loss!)
sudo lvremove vg_data/lv_1
sudo lvremove vg_data/lv_2

# Remove VG
sudo vgremove vg_data
```

---

# Logical Volumes (LV)

## Create Logical Volume

Create LV within a VG.

```bash
# Create 50GB LV
sudo lvcreate -L 50G -n lv_data vg_data

# Create LV using percentage of VG
sudo lvcreate -l 80%VG -n lv_home vg_data

# Create LV using all remaining space
sudo lvcreate -l 100%FREE -n lv_backup vg_data
```

**Expected output:**
```
Logical volume "lv_data" created.
```

## Display Logical Volumes

**Show all LVs:**
```bash
sudo lvs
```

**Expected output:**
```
  LV        VG       Attr       LSize    Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  lv_data   vg_data  -wi-ao----   50.00g
  lv_home   vg_data  -wi-ao----   40.00g
  lv_backup vg_data  -wi-ao----   10.00g
```

**Detailed LV information:**
```bash
sudo lvdisplay vg_data/lv_data
```

**Expected output:**
```
--- Logical volume ---
  LV Path                /dev/vg_data/lv_data
  LV Name                lv_data
  VG Name                vg_data
  LV UUID                lV123456-7890-aBcD-eF12-34567890aBcD
  LV Write Access        read/write
  LV Creation host, time server, 2024-01-16 10:30:45 +0000
  LV Status              available
  # open                 1
  LV Size                50.00 GiB
  Current LE             12800
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0
```

## Create Filesystem on LV

```bash
# Create ext4 filesystem
sudo mkfs.ext4 /dev/vg_data/lv_data

# Create XFS filesystem
sudo mkfs.xfs /dev/vg_data/lv_data
```

## Mount Logical Volume

```bash
# Create mount point
sudo mkdir -p /data

# Mount LV
sudo mount /dev/vg_data/lv_data /data

# Verify mount
df -h | grep /data
```

**Make mount permanent in /etc/fstab:**
```bash
/dev/vg_data/lv_data  /data  ext4  defaults  0  2
```

---

# Resizing Logical Volumes

## Extend Logical Volume

Increase LV size (non-destructive).

```bash
# Extend by 10GB
sudo lvextend -L +10G /dev/vg_data/lv_data

# Extend to specific size
sudo lvextend -L 100G /dev/vg_data/lv_data

# Extend to all available space
sudo lvextend -l +100%FREE /dev/vg_data/lv_data
```

**Expected output:**
```
Size of logical volume vg_data/lv_data changed from 50.00 GiB (12800 extents) to 60.00 GiB (15360 extents).
Logical volume vg_data/lv_data successfully resized.
```

## Resize Filesystem

After extending LV, resize the filesystem:

**For ext4:**
```bash
# Must be mounted
sudo resize2fs /dev/vg_data/lv_data

# Or specify new size
sudo resize2fs /dev/vg_data/lv_data 100G
```

**For XFS:**
```bash
# Must be mounted
sudo xfs_growfs /dev/vg_data/lv_data
# or
sudo xfs_growfs /data  # if mounted at /data
```

**Verify new size:**
```bash
df -h /data
```

## Reduce Logical Volume

**WARNING: Data loss risk! Always backup first.**

```bash
# Unmount first
sudo umount /data

# Shrink filesystem to 40GB
sudo resize2fs /dev/vg_data/lv_data 40G

# Shrink LV to 40GB
sudo lvreduce -L 40G /dev/vg_data/lv_data

# Remount
sudo mount /dev/vg_data/lv_data /data
```

---

# LVM Snapshots

Snapshots create point-in-time copies for backup or testing.

## Create Snapshot

```bash
# Create 10GB snapshot of lv_data
sudo lvcreate -L 10G -s -n lv_data_snap /dev/vg_data/lv_data
```

**Parameters:**
- `-L 10G` - Snapshot size
- `-s` - Create snapshot
- `-n` - Snapshot name
- Last parameter - Source LV

**Expected output:**
```
Logical volume "lv_data_snap" created.
```

## Use Snapshot

**Mount snapshot:**
```bash
# Create mount point
sudo mkdir /mnt/snapshot

# Mount snapshot (read-only)
sudo mount -o ro /dev/vg_data/lv_data_snap /mnt/snapshot

# Backup from snapshot
tar -czf /backups/data-backup.tar.gz /mnt/snapshot
```

**Advantages:**
- Original data remains unchanged
- Consistent backup point
- No downtime during backup

## Monitor Snapshot

```bash
# Check snapshot usage
sudo lvs -o lv_name,lv_size,snap_percent vg_data

# Example output:
# LV              LSize    Snap%
# lv_data         50.00g
# lv_data_snap    10.00g  25.00
```

## Remove Snapshot

```bash
# Unmount first
sudo umount /mnt/snapshot

# Remove snapshot
sudo lvremove vg_data/lv_data_snap
```

---

# LVM Recovery Procedures

## Recover Lost LV

If an LV was accidentally deleted but data is still on disk:

```bash
# Find LV by name
sudo pvs -o pv_name,pe_start,pe_count

# Try to recover using lvrestore or reconstruct
sudo lvcreate -L 50G -n lv_recovered vg_data
```

## Restore from Snapshot

```bash
# If corruption occurred:
# 1. Create snapshot before corruption
# 2. Mount snapshot
# 3. Copy data from snapshot
cp -r /mnt/snapshot/* /restored-data/
```

## PV Recovery

If a PV fails:

```bash
# Check status
sudo pvs -a

# Try to recover data from another PV
sudo pvmove /dev/sdb1 /dev/sdc1
```

---

# Before/After Example: Expanding Storage

### Before - Storage full:
```bash
df -h
# /data          50G  48G  2G  96% /data (almost full!)

lvs
# lv_data       50.00g
```

### Steps to expand:
```bash
# 1. Add new disk to server
# 2. Create PV
sudo pvcreate /dev/sdd

# 3. Extend VG
sudo vgextend vg_data /dev/sdd

# 4. Extend LV
sudo lvextend -L +100G /dev/vg_data/lv_data

# 5. Resize filesystem
sudo resize2fs /dev/vg_data/lv_data
```

### After - Storage expanded:
```bash
df -h
# /data          149G  48G  101G  33% /data (plenty of space!)

lvs
# lv_data       150.00g

vgs
# vg_data       150.00g 101.00g
```

---

# LVM Best Practices

1. **Plan your layout** - Separate /home, /var, /opt in different LVs
2. **Use meaningful names** - Clearly identify LV purpose
3. **Monitor space** - Alert at 80% capacity
4. **Regular snapshots** - Create before major changes
5. **Test recovery** - Practice restoring from snapshots
6. **Document setup** - Keep diagrams of PV/VG/LV layout
7. **Use multiple PVs** - Distributes load, improves redundancy
8. **Leave free space** - Keep 10-20% free in VG for flexibility
9. **Consistent PE size** - Use 4MB PE for normal workloads
10. **Backup metadata** - `vgcfgbackup` regularly

---

# Useful Commands Reference

```bash
# Physical Volumes
sudo pvs                                    # List PVs
sudo pvdisplay                              # Detailed PV info
sudo pvcreate /dev/sdb1                     # Create PV
sudo pvremove /dev/sdb1                     # Remove PV

# Volume Groups
sudo vgs                                    # List VGs
sudo vgdisplay vg_data                      # Detailed VG info
sudo vgcreate vg_data /dev/sdb1             # Create VG
sudo vgextend vg_data /dev/sdc1             # Add PV to VG
sudo vgreduce vg_data /dev/sdb1             # Remove PV from VG
sudo vgremove vg_data                       # Remove VG

# Logical Volumes
sudo lvs                                    # List LVs
sudo lvdisplay vg_data/lv_data              # Detailed LV info
sudo lvcreate -L 50G -n lv_data vg_data     # Create LV
sudo lvextend -L +10G /dev/vg_data/lv_data  # Extend LV
sudo lvreduce -L -10G /dev/vg_data/lv_data  # Reduce LV
sudo lvremove vg_data/lv_data               # Remove LV
sudo lvcreate -L 10G -s -n snap vg_data/lv  # Create snapshot
```

---

# Summary

LVM provides flexible, powerful storage management through logical volumes. It allows non-destructive resizing, snapshots for backups, and complex storage configurations. Understanding the PV/VG/LV hierarchy is key to using LVM effectively.