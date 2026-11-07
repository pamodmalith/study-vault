### Step 1: Identify your SSD

First, we need to find the **UUID** (Universally Unique Identifier) of your SSD partition. This ensures the system always mounts the correct physical drive, even if you plug in other USB sticks.

1. Open your terminal.
2. Run the following command:
```bash
lsblk -f
```
1. Look for your SSD in the list. It will likely be listed as `sda1`, `sdb1`, or `nvme1n1p1` (depending on the drive).
2. Copy the **UUID** string (it looks like `a1b2-c3d4...`) and note the **FSTYPE** (e.g., `ext4`, `btrfs`, `ntfs`, or `exfat`).

### Step 2: Create a Permanent Mount Point

You need to create the folder where this drive will "live." Common locations are under `/mnt` or inside your home folder.

Run this command (replace `my_data` with whatever name you prefer):

Bash

```bash
sudo mkdir -p /mnt/data_drive
```

### Step 3: Edit the fstab file

**Warning:** Be careful here. A bad `fstab` file can make the system fail to boot. We will test it before rebooting to be safe.

1. Open the fstab file in a text editor (Nano is easiest):
```bash
sudo nano /etc/fstab
```
2. Use the arrow keys to move to the very bottom of the file.
3. Add a new line with your drive's details. The format is:
	`[UUID] [Mount Point] [Filesystem] [Options] [Dump] [Pass]` 
	**If your drive is Linux format (ext4, btrfs, xfs):**
```plaintext
UUID=your-uuid-here  /mnt/data_drive  ext4  defaults  0  2
```
*(Replace `ext4` with your actual filesystem type from Step 1).*
Ex:`ntfs-3g`
**If your drive is Windows format (ntfs/exfat):**You need specific permissions so you can write to it without root.
```
UUID=your-uuid-here  /mnt/data_drive  ntfs-3g  uid=1000,gid=1000,umask=0022,x-gvfs-show  0  0
```
4. Press `Ctrl + O`, then `Enter` to save.
5. Press `Ctrl + X` to exit.

### Step 4: Test and Set Permissions

**Do not reboot yet!** We must check if the configuration works.

1. Run this command to mount everything in fstab:
```bash
sudo mount -a
```
- If you see **no errors**, it worked!
- If you see an error, go back and check your syntax in `/etc/fstab`.
2. **Crucial Final Step (For Linux Filesystems only):** If you used `ext4` or `xfs`, the folder is currently owned by "root," so you won't be able to save files there. Change ownership to your user:
```bash
sudo chown -R $USER:$USER /mnt/data_drive
```

Now, every time you restart your computer, your SSD will automatically be available at `/mnt/data_drive`.

---
# SymLink to home folder
### Step 1: Create the Link to Your Home Directory

This is usually the best place for it, as it will show up in your file manager (Nautilus) sidebar automatically.

1. Open your terminal.
2. Run this command (replace `/mnt/data_drive` with the actual folder name you created in the previous step):
```bash
ln -s /mnt/data_drive ~/SSD_Data
```
- `ln -s` tells Linux to make a "Soft" link.
- `/mnt/data_drive` is the **source** (where the drive actually lives).
- `~/SSD_Data` is the **destination** (the name of the shortcut you are creating in your Home folder).
### Step 2: Create a Link to Your Desktop (Optional)
If you prefer to see the drive icon right on your wallpaper (if your Fedora setup supports desktop icons):

```bash
ln -s /mnt/data_drive ~/Desktop/SSD_Data
```
### How to verify it worked
1. Open your "Files" app (Nautilus).
2. Go to your **Home** folder.
3. You should see a folder named **SSD\_Data** (or whatever you named it) with a small arrow emblem on the icon.
4. Double-click it. It should take you straight into your SSD contents.

This path will **never change**, even if you plug in 5 other USB drives, because it is tied to the unique UUID we set up in the `fstab` file earlier.

---
### 1. Can you mount directly to ~/ssd?

**Yes, absolutely.** You can mount your drive directly to a folder inside your home directory (e.g., `/home/yourname/ssd`). This is actually very common for developers.

However, there are two **critical rules** to avoid conflicts:

1. **The folder MUST be empty:** If you create the folder `~/ssd` and put files in it *before* mounting the drive, those files will disappear (become hidden) once the drive is mounted. They aren't deleted, but you can't see them until you unmount the drive.
	- *Solution:* Just make sure the folder you are mounting to is an empty directory.
2. **Do NOT use the `~` symbol in `/etc/fstab`:** The `fstab` file is read by the system before your user logs in, so it doesn't know what `~` means. You **must** use the full path.
	- *Incorrect:*`~/ssd`
	- *Correct:*`/home/pamod/ssd` (or whatever your exact username path is).

---

### 2\. GUI Tool: GNOME Disks

There is a fantastic built-in tool called **GNOME Disks** (or simply "Disks"). It writes the `/etc/fstab` file for you safely, so you don't have to touch the text file manually.

Here is how to use it:

1. **Open "Disks":** Press the `Super` key (Windows key) and type "Disks". Click the icon that looks like a hard drive.
2. **Select your Drive:** On the left sidebar, click on your NVMe/SSD drive.
3. **Select the Partition:** In the main graphic area, click on the partition you want to mount (the big block representing your data).
4. **Open Mount Options:** Click the **Gears Icon** (usually below the partition graph) and select **Edit Mount Options...**.
5. **Disable Defaults:**
	- Turn **OFF** the switch that says "User Session Defaults".
6. **Configure the Settings:**
	- **Mount at system startup:** Check this box.
	- **Show in user interface:** Check this box (so it appears in your file manager).
	- **Mount Point:** This is where you solve your path problem. Delete what is there and type your desired path: `/home/pamod/ssd` (Replace `pamod` with your actual username).
7. **Identify As:** Set this to specific formatting (like `UUID=...`) to ensure it tracks the correct drive.
8. **Click OK:** You will be asked for your password.