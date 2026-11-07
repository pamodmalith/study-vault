## **Method 1: Using GUI (Gnome Disks / Disks Utility)**

### 🔹 Steps:

1. Open **"Disks"** from the application menu.
2. Select your **USB drive** from the left panel.
3. Click on the **gear icon ⚙️** below the partitions.
4. Select **"Format Partition"**.
5. Choose a **filesystem type** (e.g., FAT32, exFAT, NTFS, EXT4).
6. Click **"Next"**, then **"Format"**.
7. (Optional) After formatting, click the **"+" button** to create a new partition if needed.

---

## **Method 2: Using `gparted` (GUI Partition Manager)**

### 🔹 Install GParted:

```shell
sudo apt install gparted
```
### 🔹 Format:

1. Open **GParted** with sudo (or through app menu).
2. Select your **USB drive** in the top-right dropdown.
3. Right-click on each partition → **"Unmount"** (if needed).
4. Right-click → **"Delete"** all partitions.
5. Click **"Apply"** (green check icon).
6. Right-click → **"New"**, set **filesystem type**, click **"Add"**.
7. Click **"Apply"** to finalize.

---

## **Method 3: Using `mkfs` in Terminal (Recommended)**

### 📌 First, find the USB device:

```bash
lsblk
```

> Find your USB, e.g., /dev/sdb (make 100% sure this is the USB and not your system disk!)


### 🔹 Format to FAT32 (for compatibility with Windows/macOS):
```bash
sudo mkfs.vfat -F 32 /dev/sdX
```
### 🔹 Format to exFAT (large files > 4GB, Windows/macOS/Linux):
```bash
sudo mkfs.exfat -L MY_DRIVE /dev/sdX
```
> Needs the `exfatprogs` package (e.g. `sudo apt install exfatprogs`).
### 🔹 Format to EXT4 (for Linux systems):
```bash
sudo mkfs.ext4 /dev/sdX
```
### 🔹 Format to NTFS:
```bash
sudo mkfs.ntfs /dev/sdX
```
> Replace `/dev/sdX` with your actual device (not a partition like `/dev/sdb1` — use the whole drive for clean format).

For more details about [[mkfs.*]]

## **Method 4: Using `dd` (Dangerous but Clean)**

**⚠️ Warning: This method will completely wipe the drive. Be 100% sure of the device name.**
```bash
sudo dd if=/dev/zero of=/dev/sdX bs=1M status=progress
```
Then create a filesystem:
```bash
sudo mkfs.vfat -F 32 /dev/sdX
```

