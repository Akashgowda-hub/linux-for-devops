# Storage & LVM

LVM stands for Logical Volume Manager.

---

# LVM Components

- Physical Volume (PV)
- Volume Group (VG)
- Logical Volume (LV)

---

# Create Physical Volume

```bash
sudo pvcreate /dev/sdb
```

---

# Create Volume Group

```bash
sudo vgcreate vg_data /dev/sdb
```

---

# Create Logical Volume

```bash
sudo lvcreate -L 5G -n lv_data vg_data
```

---

# Display LVM Information

## pvs

```bash
pvs
```

---

## vgs

```bash
vgs
```

---

## lvs

```bash
lvs
```

---

# Extend Logical Volume

```bash
sudo lvextend -L +2G /dev/vg_data/lv_data
```

---

# Summary

LVM provides flexible storage management in Linux.