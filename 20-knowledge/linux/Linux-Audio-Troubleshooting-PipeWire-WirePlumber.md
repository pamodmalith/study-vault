---
title: Linux Audio Troubleshooting — PipeWire & WirePlumber
tags:
  - linux
  - audio
  - pipewire
  - wireplumber
  - troubleshooting
  - arch
  - cachyos
---

# Linux Audio Troubleshooting — PipeWire & WirePlumber

> [!info] Purpose
> A distro-independent troubleshooting guide for Linux audio problems involving **PipeWire, WirePlumber, ALSA, audio profiles, sinks, HDMI/DisplayPort, Bluetooth, headphones, and disappearing outputs**.
>
> The main goal is to diagnose the problem logically before changing configuration.

## Quick Start

If audio suddenly stops working:

1. Check whether the audio device is visible:
   ```bash
   wpctl status
   ```
2. Check the ALSA hardware layer:
   ```bash
   aplay -l
   ```
3. Check PipeWire/WirePlumber services:
   ```bash
   systemctl --user status pipewire pipewire-pulse wireplumber --no-pager
   ```
4. Check the card's available profiles:
   ```bash
   pactl list cards
   ```
5. Look for the active profile and whether the expected **Speaker / Headphones / HDMI** port exists.
6. If a profile containing the expected output exists but is not active, test switching to it:
   ```bash
   pactl set-card-profile <CARD_NAME> '<PROFILE_NAME>'
   ```
7. If the expected sink appears, set it as the default:
   ```bash
   wpctl set-default <SINK_ID>
   ```
8. Test with:
   ```bash
   speaker-test -c 2 -t wav
   ```
9. Only after identifying the cause should you make a permanent configuration change.

---

# 1. Understand the Linux Audio Stack

A useful mental model is:

```text
Physical hardware
      │
      ▼
Kernel / ALSA
      │
      ▼
PipeWire
      │
      ▼
WirePlumber
      │
      ├── profiles
      ├── ports
      ├── sinks
      └── routing / policy
      │
      ▼
Desktop / Audio UI
(Noctalia, KDE, GNOME, Waybar, etc.)
      │
      ▼
Applications
(Firefox, Steam, games, media players)
```

### Why this matters

An audio problem does **not** necessarily mean the speakers or sound card are broken.

For example:

- ALSA can see the hardware.
- PipeWire can be running.
- WirePlumber can be running.
- But WirePlumber may select a profile that does not expose the laptop speakers.
- Your desktop audio panel will then correctly show only the outputs PipeWire currently exposes.

This distinction is extremely important.

---

# 2. The Four Layers to Check

## Layer 1 — Hardware / Kernel

First determine whether Linux sees the audio controller.

```bash
lspci -nnk | grep -A3 -i audio
```

Useful information includes:

```text
Kernel driver in use:
Kernel modules:
```

You can also check:

```bash
cat /proc/asound/cards
```

### What this tells you

If the sound controller and kernel driver are present, the hardware/driver layer is at least being detected.

---

## Layer 2 — ALSA

List playback devices:

```bash
aplay -l
```

Example:

```text
card 0: ...
  device 0: HDA Analog
  device 3: HDMI
```

### Important

If you can see something such as:

```text
HDA Analog
```

then ALSA is seeing the analog audio device.

If HDMI devices also appear when a monitor is connected, that is expected.

### If ALSA does NOT see the expected device

Do not start changing PipeWire settings yet.

Investigate:

- kernel driver
- firmware
- hardware detection
- kernel logs
- device connection
- BIOS/UEFI audio settings

---

# 3. Check PipeWire and WirePlumber

Run:

```bash
systemctl --user status pipewire pipewire-pulse wireplumber --no-pager
```

You generally want all three to be active.

Check individually if needed:

```bash
systemctl --user status pipewire --no-pager
systemctl --user status pipewire-pulse --no-pager
systemctl --user status wireplumber --no-pager
```

Restarting WirePlumber is a useful **diagnostic/recovery test**:

```bash
systemctl --user restart wireplumber
```

Then:

```bash
wpctl status
```

> [!warning] Important
> Restarting WirePlumber is not automatically a permanent fix. If the same wrong profile is selected again, the underlying policy/configuration problem remains.

---

# 4. `wpctl status` — Your First Main Diagnostic

Run:

```bash
wpctl status
```

This gives a high-level PipeWire graph.

Look for:

```text
Audio
 ├─ Devices
 ├─ Sinks
 ├─ Sources
 └─ Streams
```

A **sink** is an audio output.

Examples:

```text
Speakers
Headphones
HDMI / DisplayPort
Bluetooth headphones
```

### The most important question

Ask:

> **Does the output I expect actually exist as a PipeWire sink?**

There are two very different situations.

### Situation A — Sink exists

Example:

```text
Sinks:
  82. Speaker
```

The output exists.

The problem may be:

- wrong default sink
- application routed elsewhere
- muted volume
- wrong port

### Situation B — Sink does not exist

Example:

```text
Sinks:
  HDMI
  Headphones
```

but no:

```text
Speaker
```

Then changing the default sink cannot solve the problem.

Investigate the **card profile**.

---

# 5. `wpctl status -n`

For more precise names:

```bash
wpctl status -n
```

The `-n` form is useful when you need exact object names.

This is especially helpful when using:

```bash
wpctl set-default
```

---

# 6. Find the Audio Card and Its Profiles

Use:

```bash
pactl list cards
```

This can produce a lot of output.

You are mainly looking for:

```text
Card #...
Name: alsa_card....
Profiles:
    ...
Active Profile: ...
Ports:
    ...
```

### Why profiles matter

An audio card can have multiple possible configurations.

For example:

```text
HiFi (HDMI1, HDMI2, HDMI3, Headphones, Mic1, Mic2)

HiFi (HDMI1, HDMI2, HDMI3, Mic1, Mic2, Speaker)
```

These are **not equivalent**.

One profile may expose:

```text
Headphones
HDMI
Microphones
```

while another exposes:

```text
Speaker
HDMI
Microphones
```

If the wrong profile becomes active, your laptop speakers can disappear from PipeWire even though ALSA still sees the underlying hardware.

---

# 7. Identify the Active Profile

Inside:

```bash
pactl list cards
```

look for:

```text
Active Profile:
```

Compare it with the available profiles.

Ask:

> Does the active profile actually contain the output I want?

If the laptop speaker is only available in another profile, that is a strong clue.

---

# 8. Test a Different Profile

First identify the exact card name:

```bash
pactl list cards short
```

Then identify the exact profile:

```bash
pactl list cards
```

Switch it with:

```bash
pactl set-card-profile <CARD_NAME> '<PROFILE_NAME>'
```

Example:

```bash
pactl set-card-profile \
  alsa_card.pci-0000_00_1f.3-platform-skl_hda_dsp_generic \
  'HiFi (HDMI1, HDMI2, HDMI3, Mic1, Mic2, Speaker)'
```

### What this command does

It changes the **currently active profile** for the selected audio card.

It does NOT:

- install anything
- remove anything
- change the kernel
- change Hyprland
- change Noctalia
- permanently edit your configuration

It is primarily a runtime test.

After changing the profile:

```bash
wpctl status
```

Check whether the expected sink now exists.

---

# 9. Test the Sink Directly

If the expected sink appears:

```bash
wpctl status
```

find its ID.

For example:

```text
82. Speaker
```

Then:

```bash
wpctl set-default 82
```

### What this does

It changes the current default PipeWire output.

Again, this is a runtime change.

It does not necessarily create a permanent configuration.

---

# 10. Test the Hardware

Use:

```bash
speaker-test -c 2 -t wav
```

You should hear alternating left/right test audio.

Stop it with:

```text
Ctrl+C
```

### Why this test is useful

It bypasses the question:

> "Is Firefox/Steam/Noctalia routing audio correctly?"

and tests whether the selected audio output can actually play sound.

If `speaker-test` works but an application does not, the problem is likely routing/application-related rather than basic hardware playback.

---

# 11. Check Mute and Volume

Use:

```bash
wpctl status
```

and:

```bash
wpctl get-volume @DEFAULT_AUDIO_SINK@
```

You can also inspect PipeWire/PulseAudio-compatible state with:

```bash
pactl get-sink-mute @DEFAULT_SINK@
pactl get-sink-volume @DEFAULT_SINK@
```

If necessary:

```bash
wpctl set-mute @DEFAULT_AUDIO_SINK@ 0
```

and:

```bash
wpctl set-volume @DEFAULT_AUDIO_SINK@ 50%
```

> [!warning]
> Do not immediately assume volume is the problem. If the expected sink is completely absent, volume controls are not the root cause.

---

# 12. HDMI / DisplayPort Problems

Connecting a monitor is an important diagnostic event because HDMI/DP provides an additional audio device.

When a monitor is connected, compare:

```bash
wpctl status
```

before and after.

Then disconnect the monitor and run it again.

### Useful sequence

#### A. Monitor disconnected

```bash
wpctl status
```

Save the output.

#### B. Connect monitor

Wait a few seconds:

```bash
wpctl status
```

#### C. Disconnect monitor

Wait a few seconds:

```bash
wpctl status
```

Compare all three.

Look specifically for:

- Speaker sink
- Headphone sink
- HDMI/DP sink
- default sink
- active profile

---

# 13. A Classic Profile-Switching Failure

One common pattern looks like this:

```text
Monitor disconnected
    ↓
Laptop speaker profile active
    ↓
Speaker sink exists
```

Then:

```text
Monitor connected
    ↓
WirePlumber / policy changes audio profile
    ↓
HDMI output appears
    ↓
A different card profile becomes active
    ↓
Laptop Speaker is no longer exposed
```

After disconnecting:

```text
Monitor removed
    ↓
HDMI sink disappears
    ↓
But the previous profile remains active
    ↓
Speaker does not automatically return
```

This can make it look like:

> "My speakers are broken after unplugging the monitor."

But the underlying problem may actually be:

> **The audio card is still using the wrong profile.**

---

# 14. Desktop Audio UI vs PipeWire

If you use:

- Noctalia
- KDE
- GNOME
- Waybar
- pavucontrol
- another audio control panel

remember:

> The UI normally shows what the audio server currently exposes.

For example, if:

```bash
wpctl status
```

does not show:

```text
Speaker
```

then Noctalia cannot simply select that missing sink.

This is why troubleshooting should begin with PipeWire/WirePlumber rather than assuming the desktop panel itself is broken.

---

# 15. `pactl` vs `wpctl`

Both can interact with the PipeWire audio system.

### `wpctl`

Useful for PipeWire-native operations:

```bash
wpctl status
wpctl set-default <ID>
wpctl get-volume @DEFAULT_AUDIO_SINK@
```

### `pactl`

Useful for PulseAudio-compatible control of PipeWire:

```bash
pactl list cards
pactl list sinks
pactl set-card-profile ...
```

A practical rule:

> Use `wpctl` for the PipeWire graph and sink routing; use `pactl` when you need detailed card/profile information.

---

# 16. Read-Only vs System-Changing Commands

This distinction is important.

## 🟢 Read-only / diagnostic

These inspect the system:

```bash
wpctl status
wpctl status -n
wpctl get-volume @DEFAULT_AUDIO_SINK@

pactl list cards
pactl list sinks
pactl list cards short

aplay -l

cat /proc/asound/cards

lspci -nnk | grep -A3 -i audio

systemctl --user status pipewire pipewire-pulse wireplumber --no-pager
```

They do not intentionally change the audio configuration.

---

## 🟡 Runtime changes

These change the current session:

```bash
wpctl set-default <SINK_ID>
wpctl set-volume ...
wpctl set-mute ...
pactl set-card-profile ...
systemctl --user restart wireplumber
```

These can affect the current session but are generally not permanent configuration edits.

---

## 🔴 Persistent configuration

These can survive reboots and should only be changed after diagnosis:

- WirePlumber configuration files
- PipeWire configuration
- systemd user service overrides
- distro-specific audio configuration
- custom rules/scripts

> [!warning]
> Do not create random WirePlumber rules just because an audio command works. First identify the exact profile/routing behavior causing the problem.

---

# 17. Recovery Procedure

If you need audio back quickly:

### Step 1

```bash
wpctl status
```

### Step 2

If the expected sink exists:

```bash
wpctl set-default <SINK_ID>
```

### Step 3

If the expected sink does NOT exist:

```bash
pactl list cards
```

Look for a profile containing the expected output.

### Step 4

Switch to that profile:

```bash
pactl set-card-profile <CARD_NAME> '<PROFILE_NAME>'
```

### Step 5

Check:

```bash
wpctl status
```

### Step 6

Set the newly available sink as default if necessary:

```bash
wpctl set-default <SINK_ID>
```

### Step 7

Test:

```bash
speaker-test -c 2 -t wav
```

---

# 18. If Restarting WirePlumber Does Not Fix It

Try:

```bash
systemctl --user restart wireplumber
```

Then:

```bash
wpctl status
```

If the expected output is still missing, do not keep restarting it repeatedly.

That suggests the problem is more specific than a temporarily stuck WirePlumber process.

Investigate:

```bash
pactl list cards
```

especially:

- available profiles
- active profile
- available ports

---

# 19. When to Look at Logs

If the basic state does not explain the problem:

```bash
journalctl --user -u wireplumber -b --no-pager
```

PipeWire:

```bash
journalctl --user -u pipewire -b --no-pager
```

PipeWire Pulse compatibility layer:

```bash
journalctl --user -u pipewire-pulse -b --no-pager
```

For a live test:

```bash
journalctl --user -u wireplumber -f
```

Then connect/disconnect the monitor and observe what changes.

Press:

```text
Ctrl+C
```

to stop following the log.

---

# 20. New OS / New Distro Procedure

> [!tip] Keep this section
> If you reinstall Linux someday, start here instead of repeating the entire investigation from memory.

## After installing the new OS

### 1. Identify the audio stack

```bash
systemctl --user status pipewire pipewire-pulse wireplumber --no-pager
```

### 2. Check hardware

```bash
lspci -nnk | grep -A3 -i audio
cat /proc/asound/cards
aplay -l
```

### 3. Check PipeWire

```bash
wpctl status
```

### 4. Check profiles

```bash
pactl list cards
```

### 5. Test normal playback

```bash
speaker-test -c 2 -t wav
```

### 6. Test the important hardware events

Test:

- laptop speakers
- headphones
- HDMI/DisplayPort
- monitor disconnect
- monitor reconnect
- Bluetooth if applicable

### 7. Record the working state

Save:

```bash
wpctl status
pactl list cards
```

This gives you a baseline.

---

# 21. Decision Tree

```text
Audio not working
       │
       ▼
Does ALSA see the hardware?
       │
   ┌───┴───┐
   │       │
  NO      YES
   │       │
   ▼       ▼
Driver/   Does PipeWire expose
kernel    the expected sink?
problem       │
          ┌───┴───┐
          │       │
         NO      YES
          │       │
          ▼       ▼
       Check     Check
       profiles  default sink,
       & ports   mute & routing
          │
          ▼
Does another profile
contain the missing output?
          │
      ┌───┴───┐
      │       │
     YES      NO
      │       │
      ▼       ▼
Test that   Investigate
profile     driver/firmware/
            WirePlumber rules
      │
      ▼
Does the sink appear?
      │
  ┌───┴───┐
  │       │
 YES      NO
  │       │
  ▼       ▼
Test     Continue diagnosis
speaker  with logs/state
-test
```

---

# 22. Monitor / HDMI-Specific Decision Tree

```text
Audio works
    │
    ▼
Connect HDMI/DP monitor
    │
    ▼
Audio disappears?
    │
    ▼
wpctl status
    │
    ├── Speaker still exists
    │       │
    │       └── Check default sink / routing / mute
    │
    └── Speaker disappeared
            │
            ▼
       pactl list cards
            │
            ▼
       Check Active Profile
            │
            ▼
       Is there another profile
       containing Speaker?
            │
          YES
            │
            ▼
       Test that profile
            │
            ▼
       Speaker returns?
            │
          YES
            │
            ▼
       Likely profile-selection/
       policy issue
```

---

# 23. What Happened in the CachyOS Case Study

> [!note]
> This section documents one real investigation. The procedure above is intentionally broader than this specific machine.

### Environment

- OS: CachyOS
- Desktop/compositor: Hyprland
- Shell/UI: Noctalia V5
- Audio server: PipeWire
- Session manager: WirePlumber
- Laptop: HP
- Audio codec: Realtek ALC236
- Intel Tiger Lake audio controller
- Kernel driver: `sof-audio-pci-intel-tgl`

### Initial symptom

The laptop speakers worked after reboot.

After connecting an external monitor:

- audio disappeared
- the desktop audio selector showed the monitor output
- the laptop speaker output disappeared
- disconnecting the monitor did not immediately restore the speaker
- rebooting restored it

### Important discovery

ALSA still showed the analog audio device.

PipeWire/WirePlumber also knew about the audio card.

The card had multiple profiles.

One active profile exposed HDMI and headphones/microphones but did not expose the laptop speaker.

Another profile included:

```text
Speaker
```

Switching to the Speaker-containing profile caused a Speaker sink to appear.

Then:

```bash
wpctl set-default <speaker-sink-id>
```

selected it.

Finally:

```bash
speaker-test -c 2 -t wav
```

successfully played the test sound.

### What this demonstrated

The hardware was not simply "dead."

The important issue was the **audio profile / routing state**.

Monitor hotplugging was associated with a profile change that left the laptop speaker unavailable to PipeWire.

> [!important]
> This case is an example of why you should inspect the audio graph and card profile before reinstalling packages, changing kernels, or blaming the desktop UI.

---

# 24. The Most Important Diagnostic Questions

When troubleshooting Linux audio, ask these in order:

1. **Does the kernel see the audio controller?**
2. **Does ALSA see the expected playback device?**
3. **Are PipeWire and WirePlumber running?**
4. **Does PipeWire expose the expected sink?**
5. **What profile is active?**
6. **Does another profile expose the missing output?**
7. **Is the expected sink the default sink?**
8. **Is it muted?**
9. **Can `speaker-test` play audio?**
10. **Does a hardware event such as HDMI hotplug change the profile?**
11. **Does restarting WirePlumber change anything?**
12. **Only then: what do the WirePlumber/PipeWire logs say?**

---

# 25. Commands Cheat Sheet

## Hardware

```bash
lspci -nnk | grep -A3 -i audio
cat /proc/asound/cards
aplay -l
```

## Services

```bash
systemctl --user status pipewire pipewire-pulse wireplumber --no-pager
systemctl --user restart wireplumber
```

## PipeWire

```bash
wpctl status
wpctl status -n
wpctl get-volume @DEFAULT_AUDIO_SINK@
wpctl set-default <SINK_ID>
wpctl set-mute @DEFAULT_AUDIO_SINK@ 0
```

## Card/profile information

```bash
pactl list cards
pactl list cards short
pactl list sinks
```

## Profile testing

```bash
pactl set-card-profile <CARD_NAME> '<PROFILE_NAME>'
```

## Audio testing

```bash
speaker-test -c 2 -t wav
```

## Logs

```bash
journalctl --user -u wireplumber -b --no-pager
journalctl --user -u pipewire -b --no-pager
journalctl --user -u pipewire-pulse -b --no-pager
```

---

# 26. Golden Rule

> **Do not fix what you have not identified.**

For Linux audio:

```text
Hardware
   ↓
ALSA
   ↓
PipeWire
   ↓
WirePlumber
   ↓
Sink / Profile / Routing
   ↓
Desktop UI
   ↓
Application
```

Find the first layer where reality differs from what you expect.

That is usually much faster than randomly installing packages, changing configurations, or rebooting repeatedly.

---

# 27. Personal Troubleshooting Notes

Use this section when diagnosing a future problem.

## Date

`YYYY-MM-DD`

## OS

`...`

## Desktop / WM

`...`

## PipeWire version

```bash
pipewire --version
```

## WirePlumber version

```bash
wireplumber --version
```

## Hardware

```bash
lspci -nnk | grep -A3 -i audio
```

## ALSA

```bash
aplay -l
```

## PipeWire state

```bash
wpctl status
```

## Card/profile state

```bash
pactl list cards
```

## What changed?

- [ ] Monitor connected
- [ ] Monitor disconnected
- [ ] Headphones connected
- [ ] Headphones disconnected
- [ ] Bluetooth connected
- [ ] Bluetooth disconnected
- [ ] System update
- [ ] Kernel update
- [ ] PipeWire/WirePlumber update
- [ ] New configuration
- [ ] Reboot
- [ ] Other:

## What fixed it?

```text
...
```

## Root cause

```text
...
```

## Permanent fix

```text
...
```
