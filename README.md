# How to Expand Disk Space in Linux: A Comprehensive Guide

## Introduction
Running out of disk space can be a critical issue for any system. This guide covers different scenarios and solutions for expanding disk space in Linux systems.

## Prerequisites
- Root or sudo access
- Basic knowledge of Linux commands
- Backup of important data

## Methods Based on Environment

### 1. Cloud-Based Servers (AWS, DigitalOcean, etc.)

#### Step 1: Resize in Cloud Console
```bash
# First, check current disk size
df -h
```

1. Go to your cloud provider dashboard
2. Find volume/disk settings
3. Modify volume size
4. Apply changes

#### Step 2: Extend Partition
```bash
# List block devices
lsblk

# Extend partition
sudo growpart /dev/sda 2  # Replace with your partition

# Resize filesystem
sudo resize2fs /dev/sda2  # For ext4
# OR
sudo xfs_growfs /  # For XFS
```

### 2. Virtual Machines

#### Step 1: VM Configuration
1. Shut down the VM
2. Modify disk size in hypervisor settings
3. Start the VM

#### Step 2: OS Configuration
```bash
# Verify new space
fdisk -l

# Extend partition using parted
sudo parted /dev/sda
# Then: resizepart 2 100%
# Note: Replace numbers according to your setup
```

### 3. Physical Servers

#### Option A: Add New Disk
```bash
# Identify new disk
sudo fdisk -l

# Create partition
sudo fdisk /dev/sdb

# Format partition
sudo mkfs.ext4 /dev/sdb1

# Mount disk
sudo mkdir /mnt/newdisk
sudo mount /dev/sdb1 /mnt/newdisk

# Add to fstab for persistence
echo "/dev/sdb1 /mnt/newdisk ext4 defaults 0 0" | sudo tee -a /etc/fstab
```

#### Option B: LVM Extension
```bash
# Extend physical volume
sudo pvcreate /dev/sdb1

# Extend volume group
sudo vgextend vg_name /dev/sdb1

# Extend logical volume
sudo lvextend -l +100%FREE /dev/vg_name/lv_name

# Resize filesystem
sudo resize2fs /dev/vg_name/lv_name
```

## Common Issues and Solutions

### 1. Verification Steps
```bash
# Check disk space
df -h

# Check partition table
sudo fdisk -l

# Check mounted filesystems
mount | grep "^/dev"
```

### 2. Troubleshooting
- Always run fsck before resizing
- Ensure sufficient backup
- Check for partition alignment
- Verify filesystem type before resizing

## Best Practices
1. Always backup before resizing
2. Use LVM when possible
3. Monitor disk usage regularly
4. Plan ahead for storage needs
5. Document all changes

## Safety Precautions
- Never modify partitions on running system
- Double-check device names
- Keep rescue media handy
- Test procedure in non-production first

