---
title: Fixing Default File Manager on Hyprland (Thunar vs Dolphin)
tags:
  - linux
  - cachyos
  - hyprland
  - troubleshooting
  - obsidian-note
date: 2026-09-21
---

# Fixing Default File Manager on Hyprland (Thunar vs Dolphin)

## Problem Overview

When setting a default file manager using `xdg-mime` (such as setting `thunar.desktop` for `inode/directory` and `x-scheme-handler/file`), some applications (e.g., Firefox, Chromium, VS Code) continue to launch Dolphin when selecting "Open Containing Folder" or opening downloads.

### Symptoms
- `xdg-open ~` launches **Thunar** as expected.
- `xdg-mime query default inode/directory` outputs `thunar.desktop`.
- Opening folders from Firefox or other GUI applications still triggers **Dolphin**.
- Attempting `sudo pacman -Rns dolphin` fails due to meta-package dependencies:
  ```text
  error: failed to prepare transaction (could not satisfy dependencies)
  :: removing dolphin breaks dependency 'dolphin' required by cachyos-hypr-noctalia
  ```

---

## Root Cause: D-Bus `FileManager1` Service

Modern Linux desktop applications (especially web browsers and sandboxed software) do not invoke shell tools like `xdg-open`. Instead, they query the session D-Bus interface:

$$\text{org.freedesktop.FileManager1}$$

When multiple file managers are installed, each registers a `.service` definition inside `/usr/share/dbus-1/services/`. D-Bus activates the default or highest-priority registered handler (frequently Dolphin if KDE packages are installed).

---

## Diagnostic Commands

### 1. Verify MIME associations
```bash
xdg-mime query default inode/directory
xdg-mime query default x-scheme-handler/file
```
Both should return `thunar.desktop`.

### 2. Inspect registered D-Bus file manager services
```bash
grep -rn "org.freedesktop.FileManager1" /usr/share/dbus-1/services/
```

### 3. Test direct D-Bus invocation
```bash
busctl --user call org.freedesktop.FileManager1 /org/freedesktop/FileManager1 org.freedesktop.FileManager1 ShowFolders as 1 "file://$HOME" s ""
```
*If this command opens Dolphin despite correct `xdg-mime` settings, D-Bus service ownership is the culprit.*

---

## Solution 1: Override D-Bus Activation (Recommended)

Keep Dolphin installed (preserving meta-package dependencies like `cachyos-hypr-noctalia`) while permanently forcing D-Bus requests to route to Thunar.

### Step 1: Create user-level D-Bus override directory
```bash
mkdir -p ~/.local/share/dbus-1/services
```

### Step 2: Point `org.freedesktop.FileManager1` to Thunar
```bash
cat << 'EOF' > ~/.local/share/dbus-1/services/org.freedesktop.FileManager1.service
[D-BUS Service]
Name=org.freedesktop.FileManager1
Exec=/usr/bin/thunar --gapplication-service
EOF
```

### Step 3: Mask Dolphin's system D-Bus service file
Masking the service file prevents Dolphin's internal activator from claiming the interface:
```bash
ln -sf /dev/null ~/.local/share/dbus-1/services/org.kde.dolphin.FileManager1.service
```

### Step 4: Terminate active Dolphin processes
```bash
killall dolphin 2>/dev/null
```

### Step 5: Test the route
```bash
busctl --user call org.freedesktop.FileManager1 /org/freedesktop/FileManager1 org.freedesktop.FileManager1 ShowFolders as 1 "file://$HOME" s ""
```
Thunar should now open directly.

---

## Solution 2: Remove Dolphin by Bypassing Meta-Package

If you do not want Dolphin or KDE dependencies on the system:

### Step 1: Remove the meta-package without removing dependencies
The `-Rdd` flag skips dependency resolution for this single operation:
```bash
sudo pacman -Rdd cachyos-hypr-noctalia
```
*(All configurations, wallpapers, and Noctalia themes remain intact).*

### Step 2: Remove Dolphin cleanly
```bash
sudo pacman -Rns dolphin
```

### Step 3: Mark Thunar as explicitly installed
```bash
sudo pacman -D --asexplicit thunar
```

---

## Verification & Firefox Configuration

1. In Firefox, open **Settings** (`about:preferences`) $\to$ **Applications**.
2. Verify that **File** or **Folder** is set to **Use Thunar** (or not explicitly forced to Dolphin).
3. Test by downloading any file in Firefox, clicking the downloads menu, and selecting the **folder icon** ("Open Containing Folder").