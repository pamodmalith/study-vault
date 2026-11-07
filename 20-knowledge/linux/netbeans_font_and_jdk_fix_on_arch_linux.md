---
tags: [netbeans, arch-linux, hyprland, java, sdkman, troubleshooting]
date: 2026-09-25
---

# Troubleshooting NetBeans Fonts and JDK on Arch Linux (Hyprland)

## The Problem
After freshly installing NetBeans via `pacman` on Arch Linux (running Hyprland/Wayland):
1. **Fonts are pixelated/ugly**: The IDE text looks jagged, and changing font families or zooming does not fix the issue.
2. **Incorrect Java Version**: NetBeans may default to the system OpenJDK instead of a preferred Java version installed via `mise` or `sdkman` , or it throws a "Cannot run on older versions of Java than Java 21" error if the configuration file is malformed.

## The Root Cause
* **Fonts**: NetBeans is built on Java Swing, which does not automatically inherit system font anti-aliasing settings on Linux, particularly under Wayland/XWayland.
* **JDK Error**: When overriding the JDK path, using command-line syntax (like `-jdkhome=/path/`) inside the configuration file breaks the launch script, causing it to fall back to the default (and potentially incompatible) system Java version.

---

## Step-by-Step Solution

Instead of modifying system files with `sudo`, it is best practice to use a user-level configuration file. 

### 1. Create a User-Level Config File
NetBeans looks for a user-specific configuration file inside `~/.netbeans/<version>/etc/`.

Create the directory and copy the global configuration file:
```bash
mkdir -p ~/.netbeans/30/etc
cp /usr/share/netbeans/etc/netbeans.conf ~/.netbeans/30/etc/netbeans.conf
```

If the copy command reports `cannot stat '/usr/share/netbeans/etc/netbeans.conf': No such file or directory`, locate the actual configuration file first:

```bash
fd netbeans.conf /
```

Then replace `/usr/share/netbeans/etc/netbeans.conf` in the `cp` command with the path shown by `fd`, and run the command again.

*(Note: Replace `30` with your actual NetBeans version number if it updates in the future).*
### 2. Fix Font Rendering (Anti-Aliasing)
Open the newly copied configuration file in a text editor:
```bash
nano ~/.netbeans/30/etc/netbeans.conf
```

Find the line that starts with `netbeans_default_options=`. It contains a long string of arguments wrapped in quotes.
At the very end of this list, **inside the closing quote**, add a space and paste the following JVM arguments:
```text
-J-Dawt.useSystemAAFontSettings=on -J-Dswing.aatext=true
```
*Example:*
`netbeans_default_options="...[existing flags]... -J-Dawt.useSystemAAFontSettings=on -J-Dswing.aatext=true"`

### 3. Point NetBeans to SDKMAN Java
To force NetBeans to use a specific Java version installed via SDKMAN (e.g., Java 21), find the JDK path variable in the same `netbeans.conf` file.

Look for the commented-out line:
```text
#netbeans_jdkhome="/path/to/jdk"
```
Uncomment it (remove the `#`) and set it to your SDKMAN path. 
**CRITICAL:** Use the exact variable name `netbeans_jdkhome` with an underscore. Do *not* use `-jdkhome=`.

```text
netbeans_jdkhome="/home/pamod/.sdkman/candidates/java/21.0.11-tem"
```

### 4. Restart NetBeans
Save the file and launch NetBeans from your terminal or application launcher. The IDE will now boot using the specified Java 21 environment and fonts will render smoothly.

---

## Common Pitfalls
* **Syntax Error Crash:** If you see an error like `/home/pamod/.netbeans/30/etc/netbeans.conf: line 1: -jdkhome=... No such file or directory`, it means you pasted command-line arguments directly into the config file instead of using the proper variable syntax. Overwrite the file with the clean global template again and re-apply the correct variables.