# U1-04c — Installing Debian 13 in VirtualBox

This is the VM you'll be using for basically the rest of the course, so it's worth doing properly the first time instead of rushing through it. Read the installer screens as you go instead of clicking "Continue" on autopilot — a few of the choices below matter more than they look like they do.

---

## Before you touch anything

You'll want:
- 8 GB RAM minimum (16 GB is a lot more comfortable — the VM alone eats 2–4 GB)
- 40 GB of free disk space
- A 64-bit CPU with virtualization support (basically any laptop from the last ten years)

**On Apple Silicon (M1/M2/M3/M4):** VirtualBox does technically run, but it's shakier than on Intel/AMD machines and some ISOs won't boot at all. If you run into a wall, switch to UTM (mac.getutm.app) with the Debian **arm64** netinst ISO instead — same steps, slightly different menus. Ask if you're not sure which way to go.

**Make sure virtualization is actually turned on** — this is the single most common reason the VM refuses to start or crawls along at 2 fps:
- **Windows:** Ctrl+Shift+Esc → Performance tab → CPU → bottom right should say "Virtualization: Enabled." If it says disabled, you'll need to reboot into BIOS/UEFI and flip on Intel VT-x or AMD-V. If you've got Docker Desktop or WSL installed, Hyper-V might also be fighting with VirtualBox — turn it off under "Turn Windows features on or off" and reboot.
- **macOS:** run `sysctl kern.hv_support` in Terminal — you want a `1` back.
- **Linux:** run `grep -E 'vmx|svm' /proc/cpuinfo | head -1` — any output at all means you're fine.

---

## 1. Grab the software

- **VirtualBox 7.2.x** plus the matching **Extension Pack**, both from virtualbox.org/wiki/Downloads — official site only, don't grab it from a random mirror. Install VirtualBox with the defaults, then go to Tools → Extension Pack Manager and install the pack you downloaded.
- **Debian 13 amd64 netinst ISO** from debian.org/download — something like `debian-13.6.0-amd64-netinst.iso`. Any 13.x point release is fine; it's a small ~700 MB installer that pulls the rest down during setup.

Worth doing but not mandatory — check the ISO against the `SHA256SUMS` file on the same download page:
```bash
shasum -a 256 debian-13.6.0-amd64-netinst.iso        # mac / linux
Get-FileHash -Algorithm SHA256 debian-13.6.0-amd64-netinst.iso   # windows
```
If the hash doesn't match, the download's corrupted — grab it again. (We'll dig into what hashes are actually doing later in U2-01; consider this a preview.)

---

## 2. Build the VM

In VirtualBox, click **New**.

- Name: `debian-lab`
- ISO: point it at the file you downloaded
- Type / Version: Linux / Debian (64-bit) — it usually figures this out on its own
- Tick **Skip Unattended Installation**. This matters — VirtualBox's automatic installer skips right past the screens you're supposed to see and think about. We want the manual version.

**Hardware:**
- RAM: 2048 MB if your host has 8 GB, 4096 MB if you've got 16 GB or more
- CPUs: 2 is a sensible default
- Rule of thumb either way: never give the VM more than half your host's RAM or cores

**Disk:**
- 20–30 GB, "Create a Virtual Hard Disk Now"
- Leave "Pre-allocate Full Size" **unchecked** so it grows as needed instead of grabbing the full amount up front

**Before you hit start, open Settings on the new VM and check:**
- System → Motherboard: Optical (the ISO) should come before Hard Disk in the boot order
- Display → Screen: bump Video Memory up to 128 MB (the 16 MB default makes everything feel sluggish)
- Network → Adapter 1: NAT is fine to start with
- General → Advanced → Shared Clipboard: set to Bidirectional (won't fully kick in until Guest Additions is installed later)

---

## 3. Actually installing Debian

Start the VM and pick **Graphical Install** from the boot menu (text mode exists and you'll use it eventually when SSHing into real servers, but not today).

Walk through the screens — most things can stay default, but pay attention to these:

- Language: English | Country: Finland | Keyboard: whatever actually matches your keyboard
- Hostname: `debian-lab` | Domain name: leave it blank
- **Root password: leave it blank and click Continue.** This is the one choice in the whole installer that actually matters. Leaving it blank disables the root account entirely and sets your new user up to use `sudo` instead — which is how Ubuntu, macOS, and every cloud VM you'll ever touch does it. You *can* set a root password if you want, but you'd be doing it the old way, and everything from here on assumes you didn't.
- Full name: whatever you like | Username: `varia`, or your own name in lowercase — this is your login
- User password: pick something you'll actually remember and can type without fumbling (it's a lab VM, not your bank login — don't use `1234`, but don't overthink it either)
- Time zone: Europe/Helsinki
- Partitioning: **Guided — use entire disk** → "All files in one partition (recommended for new users)" → review the summary it shows you → Continue → **"Yes, write changes to disk"**

The base system takes 5–15 minutes to install depending on your connection.

**Software selection** — check these:
- Debian desktop environment (pulls in GNOME) — or swap for XFCE if your machine is light on RAM
- SSH server (you'll need this later)
- Standard system utilities

If you already live in the terminal and don't want the overhead of a desktop, you can skip it and just take SSH server + standard utilities — everything in this course works from the command line anyway.

Software install takes another 10–30 minutes — good time for a coffee.

When it asks about GRUB, install it to the main disk (it'll usually show something like `/dev/sda`) and say yes. The installer ejects the virtual disc and reboots on its own.

Log in with the user you just created. Welcome to Linux.

---

## 4. Once it's booted

**Update everything first:**
```bash
sudo apt update
sudo apt upgrade
```
(Type your password when prompted — nothing will appear on screen as you type, that's normal, not broken.)

**Check you actually have internet:**
```bash
ping -c 3 debian.org
ip a
```
Look for the `inet` line under an interface like `enp0s3` (ignore `lo`) — you'll typically see something in the `10.0.2.x` range. That's VirtualBox's NAT address, and it's only reachable from inside the VM — that's the whole point of NAT.

**Install Guest Additions** — without these the VM feels sluggish and the window won't resize properly:
```bash
sudo apt install build-essential dkms linux-headers-$(uname -r)
```
Then from the VM window's menu bar: **Devices → Insert Guest Additions CD image...**, and back in the terminal:
```bash
cd /media/cdrom
sudo sh VBoxLinuxAdditions.run
sudo reboot
```
(If `/media/cdrom` turns up empty, look under `/media/YOUR_USERNAME/VBox_GAs_*/` instead.)

After it reboots, try resizing the VM window — if the desktop resizes along with it, Guest Additions installed correctly.

**Take a snapshot.** This is your save point — do it now while everything works:
Shut the VM down (`sudo poweroff`), then in VirtualBox right-click the VM → Snapshots → the camera icon → name it something like `clean-install-guest-additions`. If you break the VM later (you will, at some point), you roll back to this instead of reinstalling from scratch.

---

## 5. Screenshots to collect

Drop these into `unit-1/debian-vm/`:

1. `vm-settings.png` — Settings → System tab, showing your RAM/CPU allocation
2. `login-screen.png` — the login screen, your username visible
3. `terminal-uname.png` — a terminal showing the output of:
   ```bash
   uname -a
   cat /etc/debian_version
   ip a | grep inet
   ```
4. `guest-additions-working.png` — the VM window resized to something non-default, desktop filling it properly
5. `snapshot.png` — the Snapshots view with your snapshot listed

---

## 6. Fill in your VM info card

Copy this into `U1-04c_Debian_VM.md` and fill in the blanks with your own actual values. Here's what a real, filled-in one looks like (from a Dell laptop with 8 GB RAM):

```markdown
# My Debian 13 Lab VM

## VM identity
- VM name (in VirtualBox): debian-lab
- Hostname (inside Debian): debian-lab
- Debian version (from `cat /etc/debian_version`): 13.6
- Kernel version (from `uname -r`): 6.12.17-amd64

## Allocated resources
- CPU cores: 2 (host: Intel Core i5-8250U, 4 cores/8 threads total)
- RAM: 2048 MB (host has 8 GB total)
- Disk size: 20 GB
- Network mode: NAT (default)

## User account
- Username: varia
- Root account: disabled
- Sudo works: yes

## Desktop environment
- GNOME

## Guest Additions
- Installed: yes
- Version (from `VBoxClient --version` in the VM): 7.0.20

## Snapshot
- Snapshot name: clean-install-guest-additions
- Date taken: 2026-08-31

## What worked, what didn't
The root-password screen tripped me up at first — I almost typed one in out of habit before remembering the instructions said to leave it blank. Guest Additions failed to compile the first time because I forgot to install the kernel headers first; running that one command fixed it. Next time I'd allocate a bit more disk up front instead of the bare minimum.
```

---

Keep the VM handy — you'll need to boot it live for the instructor during the walkthrough.

---

## If something's broken

| What's happening | Try this |
|---|---|
| VM won't start, or crawls | Double-check virtualization is on in BIOS. On Windows, turn off Hyper-V if it's enabled. |
| Installer can't reach the Debian mirror | Confirm the network adapter is set to NAT. Restrictive school/corporate Wi-Fi can choke the netinst download — try a home connection, or grab the full DVD ISO instead. |
| Guest Additions won't compile | You probably skipped installing `linux-headers-$(uname -r)` — go back and run that first. |
| Window won't resize | Guest Additions didn't load. Reinstall it, or if the kernel's been updated since, run `sudo dpkg-reconfigure virtualbox-guest-dkms`. |

Stuck longer than 30 minutes on one thing? Ask a classmate, then ask the instructor — don't just sit there spinning your wheels.

---

## Before you call it done

- VM boots to the login screen, no errors
- You can log in as your user
- `sudo apt update` runs clean
- Root account disabled (blank password at install)
- Guest Additions installed and the window actually resizes
- Snapshot taken and shows up in VirtualBox
- All 5 screenshots saved in `unit-1/debian-vm/`
- Info card filled in
- Ready to boot it live for the walkthrough
