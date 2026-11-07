---
title: DeepSeek
source: https://chat.deepseek.com/a/chat/s/b3fe9030-66d3-422c-9326-1b1e8ef93cf3
author: 
published: 
created: 2025-06-16
tags:
  - clippings
---
To manually change the **Sinhala font** in **Linux Mint** (for both **regular apps** and **Flatpak apps**, follow these steps:

---

## Method 1: Manual Font Change (System-Wide or User-Specific)

### Step 1: Install the Sinhala Font

#### Option A: Install from a.ttf file

1. Download a Sinhala font (e.g., [Noto Sans Sinhala](https://fonts.google.com/noto/fonts?query=sinhala), [LKLUG](http://www.freefontsfamily.com/sinhala-fonts-free-download/)).
2. **For all users (system-wide):**
3. **For current user only:**

#### Option B: Install via Package Manager (if available)

```bash
sudo apt install fonts-noto-core fonts-noto-extra  # Includes Noto Sans Sinhala
fc-cache -f -v
```

---

### Step 2: Force Sinhala Text to Use the New Font

Create a **Fontconfig rule** to prioritize your Sinhala font:

1. Open/create the config file:
	```bash
	mkdir -p ~/.config/fontconfig/conf.d
	nano ~/.config/fontconfig/conf.d/50-sinhala-font.conf
	```
2. Add this (replace `Noto Sans Sinhala` with your font name):
	```xml
	<?xml version="1.0"?>
	<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
	<fontconfig>
	    <match target="pattern">
	        <test qual="any" name="lang">
	            <string>si</string> <!-- Sinhala language code -->
	        </test>
	        <test name="family">
	            <string>sans-serif</string>
	        </test>
	        <edit name="family" mode="prepend" binding="strong">
	            <string>Noto Sans Sinhala</string>
	        </edit>
	    </match>
	</fontconfig>
	```
3. **Save (`Ctrl+O`) → Exit (`Ctrl+X`)**.
4. **Apply changes**:
	```bash
	fc-cache -f -v
	```

---

### Step 3: Verify the Font

Check if the system now uses your font for Sinhala:

```bash
LANG=si fc-match sans-serif
```

Example output:

```text
NotoSansSinhala-Regular.ttf: "Noto Sans Sinhala" "Regular"
```

---

## Method 2: Change Font for Flatpak Apps (e.g., LibreOffice)

Flatpak apps **ignore system fonts** by default. To fix this:

### Step 1: Install Font in Flatpak’s Font Directory

```bash
mkdir -p ~/.var/app/org.libreoffice.LibreOffice/config/fonts/
cp ~/Downloads/NotoSansSinhala.ttf ~/.var/app/org.libreoffice.LibreOffice/config/fonts/
```

### Step 2: Apply Fontconfig Rules for Flatpak

1. **Find the app’s ID** (e.g., `org.libreoffice.LibreOffice`).
2. **Create a fontconfig file inside Flatpak**:
	```bash
	mkdir -p ~/.var/app/org.libreoffice.LibreOffice/config/fontconfig/conf.d
	cp ~/.config/fontconfig/conf.d/50-sinhala-font.conf ~/.var/app/org.libreoffice.LibreOffice/config/fontconfig/conf.d/
	```
3. **Restart the Flatpak app**:
	```bash
	flatpak kill org.libreoffice.LibreOffice
	flatpak run org.libreoffice.LibreOffice
	```

---

## Method 3: Use GNOME Tweaks (GUI Method)

1. Install **GNOME Tweaks**:
	```bash
	sudo apt install gnome-tweaks
	```
2. Open **GNOME Tweaks** → **Fonts**.
3. Change **Default Font** or **Document Font** to your preferred Sinhala font.

---

## Reverting Changes

- **Remove the fontconfig rule**:
	```bash
	rm ~/.config/fontconfig/conf.d/50-sinhala-font.conf
	```
- **Remove fonts**:
- **Update cache**:
	```bash
	fc-cache -f -v
	```

---

### Summary

| **Action**                 | **Command/File**                                   |
| -------------------------- | -------------------------------------------------- |
| Install font (system-wide) | `sudo cp font.ttf /usr/share/fonts/`               |
| Install font (user-only)   | `cp font.ttf ~/.local/share/fonts/`                |
| Force Sinhala font         | `~/.config/fontconfig/conf.d/50-sinhala-font.conf` |
| Flatpak font fix           | `~/.var/app/APP_ID/config/fonts/`                  |
| Check active font          | `LANG=si fc-match sans-serif`                      |

Now your Sinhala text should render correctly in **all apps**, including **Flatpak**! 🚀 Let me know if you need adjustments.

New chat

DeepThink (R1)

Search

AI-generated, for reference only