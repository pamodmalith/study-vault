## 📌 Force Sinhala font for your user only

1. Create folder:
```bash
mkdir -p ~/.config/fontconfig/conf.d
```
2. Create file:
```bash
nano ~/.config/fontconfig/conf.d/10-sinhala-prefer.conf
```
3. Paste 
```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
  <match>
    <test name="lang" compare="contains">
      <string>si</string>
    </test>
    <edit name="family" mode="prepend" binding="strong">
      <string>Noto Sans Sinhala</string>
      <string>Noto Serif Sinhala</string>
      <string>LKLUG</string>
    </edit>
  </match>
</fontconfig>
```
4. Save and exit
- Press `Ctrl + O` → `Enter` → `Ctrl + X`
5. Save and rebuild:
```bash
fc-cache -fv
```
---
## 📌 Force Sinhala font for system-wide
### 🔄 What it does:
- Applies Sinhala font preference to **every user** on the system, not just your user.
- Useful if:
	- You share the PC with others.
	- You want to apply it in login screens, root apps, Flatpak apps, etc.
### 🔍 Is it good?
- ✅ **Yes**, it’s safe and recommended **if you want Sinhala to look correct globally**.
- ❌ Not needed **if only you use this system** and it's already working for you.
### 🛠️ How to apply system-wide:

1. Create file:
```bash
sudo nano /etc/fonts/conf.d/10-sinhala-prefer.conf
```
2. Paste 
```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
  <match>
    <test name="lang" compare="contains">
      <string>si</string>
    </test>
    <edit name="family" mode="prepend" binding="strong">
      <string>Noto Sans Sinhala</string>
      <string>Noto Serif Sinhala</string>
      <string>LKLUG</string>
    </edit>
  </match>
</fontconfig>
```
3. Save and exit
- Press `Ctrl + O` → `Enter` → `Ctrl + X`

3. Save and exit → rebuild cache:
```bash
sudo fc-cache -fv
```
