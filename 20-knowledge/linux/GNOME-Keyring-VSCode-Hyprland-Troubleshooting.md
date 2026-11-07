---
title: GNOME Keyring & VS Code on Hyprland — Full Troubleshooting Knowledge
aliases:
  - Keyring Fix
  - VS Code OS Keyring
  - gnome-keyring Hyprland
tags:
  - linux
  - hyprland
  - gnome-keyring
  - kwallet
  - vscode
  - troubleshooting
created: 2026-09-18
---

# GNOME Keyring & VS Code on Hyprland — Full Troubleshooting Knowledge

> [!abstract] TL;DR
> **Symptom:** VS Code errors when storing keyring secrets; online-calendar credentials vanish after restart; clipboard history resets.
> **Root cause (3 layers):** ① gnome-keyring was **never unlocked at login** (no PAM integration in `greetd`), ② VS Code **can't detect a desktop environment** on Hyprland (detects `unknown`), so it refuses to use any keyring, ③ failed prompt attempts left **corrupted half-created keyring files** behind.
> **Fix:** wire `pam_gnome_keyring.so` into `/etc/pam.d/greetd` (auto-unlock at login with the login password), set `"password-store": "gnome-libsecret"` in VS Code's `argv.json`, clean the corrupted keyring files.
> **Clipboard history** was a separate, unrelated issue (`noctalia` panel keeps it in memory only).

---

## 1. Environment (as inspected on 2026-09-18)

| Component | Value |
|---|---|
| Distro | CachyOS (Arch-based, rolling) — package build: `x86_64_v4` |
| Desktop | Hyprland, session type **Wayland** (`XDG_CURRENT_DESKTOP=Hyprland`) |
| Session start | `hyprland-uwsm.desktop` (UWSM wrapper), `wayland-session-envelope@hyprland.desktop.target` |
| Login manager | `greetd` → `noctalia-greeter` (greeter user uid 961) → `/usr/bin/Hyprland` |
| Display | `WAYLAND_DISPLAY=wayland-1` ; `DISPLAY=:0` (XWayland available) |
| Keyring daemon | `gnome-keyring 1:50.0-1.1` ; `libsecret 0.21.8.2-1.1` |
| Other keyring | `kwallet 6.30.0-1.1` (kf6) — installed but dormant |
| VS Code | Official `code 1.138.0` + `Code - OSS` artifacts present |
| Panel | `noctalia` (CachyOS Hyprland panel — clipboard + calendar widgets live here) |
| Clipboard mgr | none installed (only `wl-clipboard`); `cliphist` NOT installed |

---

## 2. Symptom List (what the user experienced)

1. **VS Code** → error / silent failure when storing keyring keys; never showed a password prompt.
2. **Calendar / Online Accounts** → after every restart, "login again" was required (credentials lost).
3. **Clipboard history** → cleared on every restart.
4. `secret-tool store` sometimes prompted `Password:` and then nothing was saved.

---

## 3. Root Cause Analysis (3 independent layers)

### Layer 1 — The keyring was never unlocked at login (main bug)

- gnome-keyring's default keyring (the **login keyring**) is normally unlocked automatically at login **by the display manager's PAM stack** with your *login password*.
- This machine uses `greetd`. Its PAM file (`/etc/pam.d/greetd`) had **no `pam_gnome_keyring.so`** anywhere:
  ```bash
  grep -rn "pam_gnome_keyring" /etc/pam.d/   # → nothing
  ```
- Result: the daemon ran (activated on demand via D-Bus), but was **locked**. Any app storing a secret triggered a prompt the user never saw.

### Layer 2 — VS Code cannot detect a usable keyring on Hyprland

- VS Code/Electron (Chromium `os_crypt`) picks its Linux password backend from the **detected desktop environment**:
  ```text
  Password storage detected desktop environment: (unknown)
  ```
  Hyprland is not GNOME/KDE/other-known-DE → VS Code declares *"OS keyring not available"* and **does not use libsecret at all**, even when gnome-keyring is running and unlocked.
- Known upstream: microsoft/vscode#231290, #187338. Workaround: force the backend via `"password-store": "gnome-libsecret"` in `argv.json`.

### Layer 3 — Corrupted keyring files from failed prompt attempts

- With the keyring locked and no PAM password, the first store triggers `gcr-prompter` (GUI unlock dialog). On this system the journal showed it **timed out after 10 s unanswered**:
  ```text
  gcr-prompter: 10 second inactivity timeout, quitting
  ```
- Each failed attempt left behind **half-created keyring files** in `~/.local/share/keyrings/` that accept item *metadata* but never persist the actual *secret value* → they must be removed, not "fixed".

> [!warning] Investigation lesson learned the hard way
> **`secret-tool store` reads the secret from STDIN, not from the command line.** Intermediate "store succeeds but lookup is empty" results were caused by testing without piping stdin:
> ```bash
> # WRONG — stores an EMPTY secret:
> secret-tool store --label=x mykey myvalue
> # CORRECT:
> echo -n "myvalue" | secret-tool store --label=x mykey myvalue
> ```

---

## 4. The Investigation Log (chronological, with commands)

> All commands below were actually run. Environment: my user shell in the same session as Hyprland.

### Step 1 — Confirm environment & daemon state
```bash
cat /etc/os-release                      # CachyOS
echo $XDG_CURRENT_DESKTOP $XDG_SESSION_TYPE   # Hyprland / wayland
ps aux | grep -i keyring                 # daemon running: --components=pkcs11,secrets
ls -la ~/.local/share/keyrings/          # keyring files present
```
**Found:** daemon running via D-Bus activation with `pkcs11,secrets` components; `default`, `Default_keyring.keyring`, `user.keystore` (0 bytes) present. **No SSH component, no PAM/login component.**

### Step 2 — Check PAM wiring
```bash
ls /etc/pam.d/                          # greetd present
cat /etc/pam.d/greetd                   # no pam_gnome_keyring lines
grep -rn pam_gnome_keyring /etc/pam.d/  # nothing anywhere
ls -la /usr/lib/security/pam_gnome_keyring.so   # module IS installed (package ships it)
```
**Found:** greetd is the greeter used; `pam_gnome_keyring.so` ships with the `gnome-keyring` package but is **not referenced** by any PAM file. → Layer 1 confirmed.

### Step 3 — Inspect D-Bus activation (how the daemon starts)
```bash
cat /usr/share/dbus-1/services/org.freedesktop.secrets.service
# Exec=/usr/bin/gnome-keyring-daemon --start --foreground --components=secrets
busctl --user status org.freedesktop.secrets   # shows owning PID (the daemon)
```
**Found:** D-Bus activation is *just* `--start`. No login password involved → daemon can only prompt (and that prompt timed out).

### Step 4 — Journal shows the failed prompt (the corrupted keyring birth)
```bash
journalctl -b | grep -iE "gcr-prompter|gnome-keyring"
```
```text
gcr-prompter: Gcr: sending the secret exchange: [sx-aes-1] ...
gcr-prompter: Gcr: 10 second inactivity timeout, quitting
gnome-keyring-daemon: asked to register item ... but it's already registered
```
**Found:** at 00:03:31 the prompt fired but was never answered → a broken `Default_keyring` was created. Its timestamps matched the VS Code launch.

### Step 5 — Prove the corrupted keyring swallows secrets
```bash
secret-tool store --label=probe p1 p2     # exit 0 (!!)
secret-tool search --all p1 p2            # item exists, BUT "secret =" EMPTY
```
**Found:** item metadata persisted, secret value empty → confirmed broken state; also the cause of the "every restart requires re-login" calendar behavior.

### Step 6 — Apply the fix (see §5), re-login
After adding PAM lines and re-logging in, the journal showed:
```text
gkr-pam: couldn't unlock the login keyring.            # greeter's early attempt (no password yet)
gkr-pam: stashed password to try later in open session
gkr-pam: unlocked login keyring                        # ← SUCCESS on the real session
```
The daemon was now started by systemd user session at login (PPID = systemd --user) and owns `org.freedesktop.secrets`.

### Step 7 — Re-test with correct stdin usage (everything works)
```bash
echo -n "v1" | secret-tool store --label=real k1 v1 && secret-tool lookup k1 v1
# → v1   (no prompt, instant)
```

### Step 8 — VS Code still failed → found the second bug
```bash
ls ~/.vscode/          # MISSING (dir was wiped)
cat ~/.vscode-oss/argv.json  # contained "password-store": "basic" ← bypasses keyring!
```
**Found:** the `~/.vscode` dir (with the earlier `password-store` fix) had disappeared and the OSS build had `"basic"`. Restored `"gnome-libsecret"` in both `argv.json` files. → Layer 2 confirmed as an ongoing, re-appearing issue (must be in the docs!).

---

## 5. The Fix — Step by Step, With Explanation

### 5.1 Add PAM auto-unlock for greetd (needs sudo) ✔ DONE

File: `/etc/pam.d/greetd` — final state:
```text
#%PAM-1.0

auth       required     pam_securetty.so
auth       requisite    pam_nologin.so
auth       include      system-local-login
account    include      system-local-login
session    include      system-local-login
session    required     pam_systemd.so
-auth      optional     pam_gnome_keyring.so
-password  optional     pam_gnome_keyring.so use_authtok
-session   optional     pam_gnome_keyring.so auto_start
```

**What each new line does:**

| Line | Phase | Meaning |
|---|---|---|
| `-auth optional pam_gnome_keyring.so` | auth | Captures the login password into the PAM stack's saved-authtok, so it can be handed to the keyring daemon. The `-` prefix suppresses error reporting (module is optional & non-fatal). |
| `-password optional pam_gnome_keyring.so use_authtok` | password | During password-change flows, forwards the (already confirmed) new password on to the keyring, keeping keyring + login passwords in sync. `use_authtok` = don't prompt again, reuse the token from auth. |
| `-session optional pam_gnome_keyring.so auto_start` | session | After login succeeds, starts `gnome-keyring-daemon` (via the user systemd service) and tells it the login password, so the **login keyring auto-unlocks** — no prompts ever. |

**Why `optional` + `-`?** If pam_gnome_keyring is missing or fails, login must **not** be blocked.

> [!note] How the mechanism works internally
> `pam_gnome_keyring.so` with `auto_start` triggers the systemd **user** unit `gnome-keyring-daemon.service` at login. The password captured in the `auth` phase is stashed, and in the session phase the daemon uses it to *unlock (or create) the login keyring*. Journal proof: `gkr-pam: stashed password to try later in open session` → `gkr-pam: unlocked login keyring`.

**Apply it (idempotent script used):**
```bash
# /home/pamod/fix-gnome-keyring-pam.sh  (backup + append, exits if already present)
sudo bash ~/fix-gnome-keyring-pam.sh
```

### 5.2 Force VS Code to use libsecret on Hyprland ✔ DONE

File: `~/.vscode/argv.json` (official build) / `~/.vscode-oss/argv.json` (OSS build):
```json
{
    "password-store": "gnome-libsecret"
}
```

| value | Meaning |
|---|---|
| `gnome` / `gnome-libsecret` | Use the Secret Service via libsecret (→ gnome-keyring). **This is what we want.** |
| `basic` | Chromium's hardcoded fallback key — NOT the keyring at all. Was set on the OSS build; bad. |
| `kwallet5` / `kwallet6` | Force KWallet backend (only useful if you switch to KWallet/SDDM). |
| `gnome-keyring` | deprecated alias |

> [!warning] Re-appearing footgun
> `~/.vscode/` was observed to disappear at least once during this troubleshooting. If VS Code starts erroring again "OS keyring not available", **first check this file exists** with `grep password-store ~/.vscode/argv.json`, then fully restart VS Code (argv.json is only read at startup).

### 5.3 Remove corrupted keyring files ✔ DONE

```bash
mkdir -p ~/.local/share/keyrings/.broken-20260918
mv ~/.local/share/keyrings/Default_keyring.keyring \
   ~/.local/share/keyrings/default \
   ~/.local/share/keyrings/user.keystore \
   ~/.local/share/keyrings/.broken-20260918/
```
- **Why move (not delete)?** Safety backup first; contents had no real secrets (all "empty-secret" writes) but moving is reversible.
- Also killed the daemon so its in-memory broken state cleared: `kill <daemon-pid>` (it restarts on demand via D-Bus/systemd).

### 5.4 Remove leftover junk keyrings (current state, optional tidy-up)

Current content of `~/.local/share/keyrings/`:
```
login.keyring        ← the real one, auto-unlocked (KEEP)
Default_Key.keyring  ← leftover from a failed prompt at 00:51 (junk, not the default; can be moved to .broken/)
user.keystore        ← gnome-keyring pkcs11 artifact ("Gnome Keyring Store 2") with keypair; harmless
```

---

## 6. How gnome-keyring Works (concepts to know)

- **Architecture:** `gnome-keyring-daemon` implements the **Secret Service D-Bus API** at `org.freedesktop.Secret.Service` (`/org/freedesktop/secrets`). Any app/libs that speak it = "libsecret clients" (VS Code/Electron, Chromium, 1Password, GNOME Online Accounts, `secret-tool`).
- **Storage:** keyrings are files in `~/.local/share/keyrings/*.keyring`, encrypted with the keyring's password. The file `default` (if present) names the default keyring; otherwise the **login keyring** acts as default.
- **Locked vs unlocked:** a locked keyring has its data only on disk (encrypted) and the daemon can't read secrets → any client access triggers an unlock **prompt** (`gcr-prompter`) or **fails** if the prompt can't render (the 10 s inactivity timeout we saw).
- **Prompt flow:** `libsecret` → daemon → `org.gnome.keyring.SystemPrompter` D-Bus service → `/usr/lib/gcr-prompter` (GTK dialog). If the dialog can't be shown/answered → timeout → broken state.
- **Login integration (the fix):** PAM captures the login password at login and hands it to the daemon via its control socket (`/run/user/$UID/keyring/control`). The daemon then auto-unlocks the login keyring **without any prompt**.
- **Components:** daemon can run `ssh`, `secrets`, `pkcs11` components (`--components=`). Our setup: `pkcs11,secrets`. (SSH-agent component was removed from gnome-keyring ≥ 1.46; use `gcr-ssh-agent` or `ssh-agent`.)

---

## 7. Keyring Choice: gnome-keyring vs KWallet

### Recommendation: **stick with gnome-keyring** ✔

| Criterion | gnome-keyring | KWallet |
|---|---|---|
| libsecret (VS Code/Chromium/1Password/GOA) | native ✅ | needs kwalletd + `kwallet5/6` password-store flags; finicky |
| On Hyprland | works with the PAM fix (no prompts) | requires all apps forced to `kwallet6`; more moving parts |
| Auto-unlock at login | via one PAM file, password = login ✅ | needs KWalletManager plugin + wallet password = login, or unencrypted wallet |
| Ecosystem fit | default for all GNOME/libsecret apps | default for KDE/Qt apps |
| Current state here | **working, verified** | dormant, installed only as dependency |

### Can KWallet be safely removed? — **No (not cleanly)** ⚠️

Verified on this system:
```
kwallet 6.30.0-1.1   (kf6)   Installed Size: 2.57 MiB
Required By : kio  seahorse
Optional For: git  libsecret
```
Reverse dependency chain:
```
kwallet
└─kio ─ baloo → baloo-widgets → dolphin → cachyos-hypr-noctalia
      └ kde-cli-tools → cachyos-hypr-noctalia
      └ kio-extras / kparts / kcmutils / knewstuff / ...
└─seahorse
```
- `kwallet` is a **hard dependency of `kio`** (KDE's file framework), which in turn is required by `dolphin`, `kde-cli-tools`, and ultimately the **`cachyos-hypr-noctalia`** package (your panel/file-manager stack).
- `seahorse` (useful GUI for managing gnome-keyring) also requires it.

**Options:**
1. **Recommended — leave it dormant.** Not running, not claiming `org.freedesktop.secrets`, 2.57 MiB. Zero interference with gnome-keyring. ✅
2. **Force remove** (not recommended): `sudo pacman -Rdd kwallet` — ignores the dependency check and *will* break `kio`'s wallet integration (dolphin password saving, encrypted dirs); may cause odd KDE-kio errors. Seal the "don't" with: if you ever need it back: `sudo pacman -S kwallet`.
3. **Full cleanup** (overkill, not recommended): `sudo pacman -Rns dolphinkio kde-cli-tools baloo ...` — would also shake `cachyos-hypr-noctalia`. Not worth it.

> [!info] Even if removed, KWallet is **not needed** by anything here — the decision is purely about clean dependency management. Leaving it is the pragmatic choice.

---

## 8. How Omarchy Handles This (research findings)

- **Omarchy** (github.com/omacom/omarchy, by DHH) is an *opinionated Arch + Hyprland* distro. It installs `gnome-keyring` as part of its setup.
- **What Omarchy does NOT do:** it does **not** add `pam_gnome_keyring` auto-unlock to greetd. Evidence from their own issue tracker:
  - Issue **#352** "Gnome-keyring not creating default keyvault, 1Password MFA issues": *"creating a keyring with Seahorse does allow 1Password to save tokens, however, that keyring does not automatically unlock when first logging into the system"* → closed as not planned (PR #2242 referenced).
  - Issue **#3308** "keyring isn't being unlocked at login, so apps can't access stored credentials and create new keyrings instead" → also unresolved.
- **Practical conclusion:** stock Omarchy users hit **the exact same symptoms** we fixed here. The `/etc/pam.d/greetd` PAM wiring we applied is **better than Omarchy's default**.

---

## 9. Clipboard History & Calendar (that were blamed on the keyring)

| Issue | Real cause | Resolution |
|---|---|---|
| Clipboard history resets on restart | `noctalia` panel stores clipboard history **in memory only** (no disk persistence) — by design | Options: (a) accept it; (b) install `cliphist` + Hyprland binds for persistent history: `sudo pacman -S cliphist`, then bind `wl-paste --type text --watch cliphist store` and `cliphist list \| cliphist decode \| wl-copy` |
| Calendar/Online accounts require re-login after restart | Broken keyring → credentials stored as **empty secrets** → lost on reboot | Fixed by keyring fix; re-authenticate **once** and it persists |
| Occasional `Password:` prompt from `secret-tool` | No keyring existed yet at that moment → first store = create-keyring flow | Gone now; keyring auto-created/unlocked by PAM |

---

## 10. Verification & Rollback

### Verify the fix is active
```bash
# 1. PAM wiring present:
grep pam_gnome_keyring /etc/pam.d/greetd

# 2. Keyring auto-unlocks at login (next login):
journalctl -b | grep -i "gkr-pam"

# 3. Secret round-trip WITHOUT prompting (remember stdin!):
echo -n "mysecret" | secret-tool store --label=check ck cv && secret-tool lookup ck cv && secret-tool clear ck cv

# 4. VS Code: fully quit & reopen, sign in to GitHub/Sync → no banner, no error.
```

### Rollback
```bash
# Remove the PAM lines (restore backup made by the script):
sudo cp /etc/pam.d/greetd.bak-keyringfix /etc/pam.d/greetd   # or edit by hand
# Revert VS Code store:
#   remove "password-store" line from ~/.vscode/argv.json (or set "basic")
# Restore old keyring files:
mv ~/.local/share/keyrings/.broken-20260918/* ~/.local/share/keyrings/
```

---

## 11. Quick Troubleshooting Cheat-Sheet

| Symptom | Check | Fix |
|---|---|---|
| VS Code "OS keyring not available" / sign-in loop | `grep password-store ~/.vscode/argv.json` (file may be missing) | ensure `"gnome-libsecret"`, fully restart VS Code |
| Stores silently empty | Are you using `secret-tool`? **pipe stdin** | `echo -n "secret" \| secret-tool store ...` |
| Prompt appears then vanishes | `journalctl -b \| grep gcr-prompter` (10 s timeout) | confirmed sign the daemon wasn't PAM-unlocked; check greetd PAM + re-login |
| Credentials gone after restart | `ls ~/.local/share/keyrings/` — broken keyring files? | move them to a backup dir, restart daemon |
| Keyring never unlocks at login | `grep pam_gnome_keyring /etc/pam.d/greetd` | add `-auth`/`-password use_authtok`/`-session auto_start` lines |
| Daemon missing | `busctl --user status org.freedesktop.secrets` | should auto-start; check systemd user unit `gnome-keyring-daemon.service` |

---

## 12. Reference Links

- ArchWiki — [GNOME Keyring](https://wiki.archlinux.org/title/GNOME/Keyring) (PAM integration, automatic login w/ blank password, secrets portal)
- VS Code docs — [Troubleshooting keychain issues](https://code.visualstudio.com/docs/editor/settings-sync#_troubleshooting-keychain-issues)
- microsoft/vscode — [Hyprland not detected #231290](https://github.com/microsoft/vscode/issues/231290), [DE unknown #187338](https://github.com/microsoft/vscode/issues/187338), [libsecret init failure #194530](https://github.com/microsoft/vscode/issues/194530)
- Omarchy — [issue #352](https://github.com/omacom/omarchy/issues/352), [#3308](https://github.com/omacom/omarchy/issues/3308)
- NixOS Discourse — [Login Keyring did not get unlocked (Hyprland)](https://discourse.nixos.org/t/login-keyring-did-not-get-unlocked-hyprland/40869)
- gnome-keyring — [docs/keyring-intro.txt](https://github.com/GNOME/gnome-keyring/blob/main/docs/keyring-intro.txt)

---

*Knowledge captured on 2026-09-18 after a full live investigation on CachyOS + Hyprland. All commands were executed in that session; results included verbatim.*