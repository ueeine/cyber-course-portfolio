# Linux CLI Basics

**Goal:** Get comfortable with the everyday Debian command line — moving around, handling files, viewing and searching content, redirection, permissions, processes, networking, and installing packages.
**Source:** U1-05 Linux CLI Basics assignment
**Environment:** Debian 13 (trixie) via WSL2, user: ueeine, host: dolma

---

## Part 1 — Getting your bearings

### Q1 — What username are you logged in as?

Command:
```
$ whoami
```

Output:
```
ueeine
```

**Answer:** ueeine.

### Q2 — Are you a member of the sudo group? How can you tell from the output of id?

Command:
```
$ id
```

Output:
```
uid=1000(ueeine) gid=1000(ueeine) groups=1000(ueeine),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),100(users)
```

**Answer:** Yes — `27(sudo)` appears in the `groups=` list, confirming membership in the sudo group.

### Q3 — What kernel version is your system running?

Command:
```
$ uname -a
```

Output:
```
Linux dolma 6.18.33.2-microsoft-standard-WSL2 #1 SMP PREEMPT_DYNAMIC Thu Jun 18 21:54:43 UTC 2026 x86_64 GNU/Linux
```

**Answer:** 6.18.33.2-microsoft-standard-WSL2 (running under WSL2 rather than a standalone Hyper-V VM).

### Q4 — What is the difference in the depth of information whatis and man give you?

Command:
```
$ whatis whoami
$ man whoami
```

Output:
```
whoami (1)           - print effective user name
```
(man opened the full manual page: NAME, SYNOPSIS, DESCRIPTION, options, AUTHOR, REPORTING BUGS, SEE ALSO)

**Answer:** `whatis` is a one-line summary pulled straight from the man page. `man` is the full manual — description, every flag, and related commands. `whatis` is a reminder; `man` is the reference.

### Q5 — While in man, how do you (a) search for "user" and (b) quit?

**Answer:** Type `/user` and press Enter to jump to the first match, `n` to jump to the next match. Press `q` to quit.

---

## Part 2 — Navigation

### Q6 — What did cd - do?

Command:
```
$ cd /etc
$ pwd
$ cd ..
$ pwd
$ cd /var/log
$ pwd
$ cd -
$ pwd
$ cd ~
$ pwd
$ cd /
$ pwd
```

Output:
```
/etc
/
/var/log
/
/
/home/ueeine
/
```

**Answer:** `cd -` returned to the previous working directory and printed the resulting path — useful for bouncing between two locations without retyping the full path.

### Q7 — What additional information does -l give you over plain ls?

Command:
```
$ ls -l /etc
```

Output (excerpt):
```
-rw-r--r-- 1 root root    3981 May  6  2025 adduser.conf
drwxr-xr-x 2 root root    4096 May 16 16:32 alternatives
drwxr-xr-x 3 root root    4096 Sep  2 21:34 apparmor.d
```

**Answer:** `-l` shows the long format — permissions, link count, owner, group, size, and last-modified date/time — on top of the filename. Plain `ls` is just names in a grid.

### Q8 — What does -a show that wasn't visible before? Name two examples.

Command:
```
$ ls -la /etc
```

Output (excerpt):
```
drwxr-xr-x 56 root root    4096 Sep  2 23:43 .
drwxr-xr-x 18 root root    4096 Sep  2 21:20 ..
-rw-------  1 root root       0 May 16 16:31 .pwd.lock
-rw-r--r--  1 root root     208 May 16 16:31 .updated
```

**Answer:** Hidden entries — anything starting with a dot — plus `.` and `..` for the current and parent directory. Two examples found in `/etc`: `.pwd.lock` and `.updated`.

### Q9 — What is the largest file in /var/log? What size is it?

Command:
```
$ ls -lh /var/log
```

Output:
```
total 52K
-rw-r--r--  1 root root             174 Sep  2 21:34 alternatives.log
drwxr-xr-x  2 root root            4.0K Sep  2 23:43 apt
-rw-rw----  1 root utmp               0 May 16 16:31 btmp
-rw-r--r--  1 root root             31K Sep  2 23:43 dpkg.log
drwxr-sr-x+ 3 root systemd-journal 4.0K Sep  2 21:20 journal
-rw-rw-r--  1 root utmp               0 May 16 16:31 lastlog
drwx------  2 root root            4.0K May 16 16:31 private
lrwxrwxrwx  1 root root              39 May 16 16:31 README -> ../../usr/share/doc/systemd/README.logs
-rw-rw-r--  1 root utmp             384 Sep  2 21:20 wtmp
```

**Answer:** `dpkg.log`, at 31K.

### Q10 — What was modified most recently?

Command:
```
$ ls -lt /var/log
```

Output:
```
total 52
-rw-r--r--  1 root root            31299 Sep  2 23:43 dpkg.log
drwxr-xr-x  2 root root             4096 Sep  2 23:43 apt
-rw-r--r--  1 root root              174 Sep  2 21:34 alternatives.log
-rw-rw-r--  1 root utmp              384 Sep  2 21:20 wtmp
drwxr-sr-x+ 3 root systemd-journal  4096 Sep  2 21:20 journal
drwx------  2 root root             4096 May 16 16:31 private
lrwxrwxrwx  1 root root               39 May 16 16:31 README -> ../../usr/share/doc/systemd/README.logs
-rw-rw----  1 root utmp                0 May 16 16:31 btmp
-rw-rw-r--  1 root utmp                0 May 16 16:31 lastlog
```

**Answer:** `dpkg.log`, timestamped Sep 2 23:43 — it sorts to the top since `-t` orders newest-first by modification time.

---

## Part 3 — Creating and managing files

### Q11 — Show the command(s) you used to create the directory structure.

Command:
```
$ mkdir -p ~/cyber-course/unit1 ~/cyber-course/unit2 ~/cyber-course/unit3/{osint,recon,crypto} ~/cyber-course/scratch
```

**Answer:** One line, using brace expansion for the three subfolders under `unit3` to avoid three separate `mkdir` calls.

File creation and moves:
```
$ cd ~/cyber-course/scratch
$ touch a.txt b.txt c.txt
$ ls
a.txt  b.txt  c.txt
$ cp a.txt ~/cyber-course/unit1/intro.txt
$ ls ~/cyber-course/unit1/
intro.txt
$ mv b.txt ~/cyber-course/unit2/
$ mv c.txt notes.txt
$ ls
a.txt  notes.txt
```

### Q12 — What key combination did you use to save? What key combination did you use to exit?

Command:
```
$ nano ~/cyber-course/unit1/intro.txt
```
(typed the two sentences from the instructions)

```
$ cat ~/cyber-course/unit1/intro.txt
```

Output:
```
This is my first file edited from the Linux command line.
Today I learned that mv is also rename, and that nano shows shortcuts at the bottom.
```

**Answer:** `Ctrl+O` (Write Out) to save — confirms the filename, then Enter. `Ctrl+X` to exit.

### Q13 — Why did rmdir fail (or succeed)?

Command:
```
$ rmdir ~/cyber-course/scratch
```

Output:
```
rmdir: failed to remove '/home/ueeine/cyber-course/scratch': Directory not empty
```

**Answer:** `rmdir` only removes directories that are completely empty. At this point `scratch/` still contained `notes.txt`, so it errored out with "Directory not empty."

To actually clear it out:
```
$ rm -r ~/cyber-course/scratch
```

---

## Part 4 — Viewing files

### Q14 — Which Debian version do you have?

Command:
```
$ cat /etc/os-release
```

Output:
```
PRETTY_NAME="Debian GNU/Linux 13 (trixie)"
NAME="Debian GNU/Linux"
VERSION_ID="13"
VERSION="13 (trixie)"
VERSION_CODENAME=trixie
DEBIAN_VERSION_FULL=13.6
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
```

**Answer:** Debian GNU/Linux 13 (trixie).

Also practiced:
```
$ cat /etc/services      # long file, viewed with cat then Ctrl+C
$ less /etc/services     # scrolled with arrows, searched with /ssh, n for next, G/g for end/start, q to quit
$ head -n 5 /etc/services
```

Output of head:
```
# Network services, Internet style
#
# Updated from https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml .
#
# New ports will be added on request if they have been officially assigned
```

### Q15 — What kind of messages do you see? Are they recent?

Note: this system has no `/var/log/syslog` (WSL2 uses systemd's journal instead — `/var/log` only contains `dpkg.log`, `alternatives.log`, and the `journal/` directory). Used `journalctl` in its place.

Command:
```
$ sudo journalctl -n 10
```

Output:
```
Sep 02 23:43:12 dolma sudo[1364]: pam_unix(sudo:session): session closed for user root
Sep 02 23:43:25 dolma sudo[1412]:   ueeine : TTY=pts/2 ; PWD=/home/ueeine/test-extract ; USER=root ; COMMAND=/usr/bin/apt...
Sep 02 23:43:25 dolma sudo[1412]: pam_unix(sudo:session): session opened for user root(uid=0) by (uid=1000)
Sep 02 23:43:25 dolma sudo[1412]: pam_unix(sudo:session): session closed for user root
Sep 02 23:44:10 dolma kernel: mini_init (120): drop_caches: 1
Sep 03 00:00:48 dolma systemd[1]: Starting dpkg-db-backup.service - Daily dpkg database backup service...
Sep 03 00:00:48 dolma systemd[1]: dpkg-db-backup.service: Deactivated successfully.
Sep 03 00:00:48 dolma systemd[1]: Finished dpkg-db-backup.service - Daily dpkg database backup service.
Sep 03 00:05:30 dolma sudo[1575]:   ueeine : TTY=pts/2 ; USER=root ; COMMAND=/usr/bin/journalctl -n 10
Sep 03 00:05:30 dolma sudo[1575]: pam_unix(sudo:session): session opened for user root(uid=0) by (uid=1000)
```

**Answer:** Mostly routine system activity — sudo session opens/closes, a kernel cache-drop notice, and a scheduled `dpkg-db-backup.service` run, each timestamped. Yes, they're recent — entries span Sep 2 23:43 through Sep 3 00:05, right up to when the command was run.

---

## Part 5 — Searching

### Q16 — How many lines were returned?

Command:
```
$ grep "ssh" /etc/services
$ grep "ssh" /etc/services | wc -l
```

Output:
```
ssh             22/tcp                          # SSH Remote Login Protocol
1
```

**Answer:** 1 line.

### Search for "error" (case-insensitive) — substituted for syslog since it doesn't exist on this system

Command:
```
$ sudo journalctl | grep -i "error"
```

Output:
```
Sep 02 21:20:15 dolma kernel: RAS: Correctable Errors collector initialized.
Sep 02 21:20:58 dolma unknown: WSL (135) ERROR: CheckConnection: getaddrinfo() failed: -5
Sep 02 23:25:50 dolma sudo[762]:   ueeine : TTY=pts/2 ; USER=root ; COMMAND=/usr/bin/grep -i error /var/log/syslog
```

### Q17 — How would you modify the command to show only .conf files modified in the last 7 days?

Command:
```
$ find /etc -name "*.conf" -mtime -7
```

Output:
```
/etc/resolv.conf
/etc/ld.so.conf.d/ld.wsl.conf
```
(ran with `sudo` to avoid `Permission denied` errors on restricted directories like `/etc/ssl/private` and `/etc/credstore`)

**Answer:** `-mtime -7` restricts results to files modified less than 7 days ago.

### Q18 — Where are these commands actually located on the filesystem?

Command:
```
$ which ls
$ which nano
```

Output:
```
/usr/bin/ls
/usr/bin/nano
```

**Answer:** `/usr/bin/ls` and `/usr/bin/nano`.

---

## Part 6 — History, redirection, and pipes

### Q19 — What does the | symbol do here?

Command:
```
$ history | tail -n 20
```

**Answer:** `|` is a pipe — it feeds the output of the command on the left as input to the command on the right, letting commands chain together instead of dumping to the screen or a temp file.

### Q20 — What is the difference between > and >>?

Command:
```
$ ls -la ~/cyber-course/ > ~/listing.txt
$ cat ~/listing.txt
$ date >> ~/listing.txt
$ cat ~/listing.txt
```

Output (first cat):
```
total 68
drwxr-xr-x 6 ueeine ueeine  4096 Sep  3 00:02 .
drwx------ 7 ueeine ueeine  4096 Sep  3 00:04 ..
-rw-r--r-- 1 ueeine ueeine 15856 Sep  2 23:41 debian2.html
-rw-r--r-- 1 ueeine ueeine 15856 Sep  2 02:27 debian.html
-rwx------ 1 ueeine ueeine    47 Sep  2 23:29 hello.sh
drwxr-xr-x 2 ueeine ueeine  4096 Sep  3 00:02 scratch
drwxr-xr-x 2 ueeine ueeine  4096 Sep  3 00:00 unit1
-rw-r--r-- 1 ueeine ueeine   430 Sep  2 23:28 unit1.zip
drwxr-xr-x 2 ueeine ueeine  4096 Sep  2 23:57 unit2
-rw-r--r-- 1 ueeine ueeine   145 Sep  2 23:27 unit2.tar.gz
drwxr-xr-x 5 ueeine ueeine  4096 Sep  2 21:38 unit3
```

Output (second cat, after `date >>`):
```
(same listing as above)
Thu Sep  3 12:11:33 AM EEST 2026
```

**Answer:** `>` overwrites the file with just the new output. `>>` appends new output to the end, leaving what's already there intact — confirmed since the second `cat` showed the original listing plus the `date` output tacked on.

### Q21 — What was the output, and why?

Command:
```
$ history | grep "cd "
$ echo "hello cyber world" | grep "cyber"
```

Output:
```
hello cyber world
```

**Answer:** `hello cyber world` — `echo` prints that literal string, and since it contains "cyber," `grep` lets the whole line through unchanged.

### clear vs Ctrl+L

Ran `clear`, then `Ctrl+L` on a new line.

**Answer:** Both wipe the visible screen, but `Ctrl+L` is a built-in shortcut the terminal handles directly, while `clear` launches a small external program to do the same thing. Neither actually deletes scrollback history — it's just scrolled out of view.

---

## Part 7 — Archives

Command:
```
$ cd ~/cyber-course/
$ zip -r unit1.zip unit1/
$ unzip -l unit1.zip
```

Output:
```
Archive:  unit1.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
        0  2026-09-02 22:29   unit1/
      143  2026-09-02 22:28   unit1/intro.txt
---------                     -------
      143                     2 files
```

Command:
```
$ mkdir ~/test-extract
$ cd ~/test-extract
$ unzip ~/cyber-course/unit1.zip
$ ls -la
```

Output:
```
Archive:  /home/ueeine/cyber-course/unit1.zip
  inflating: unit1/intro.txt
total 12
drwxr-xr-x 3 ueeine ueeine 4096 Sep  2 23:28 .
drwx------ 7 ueeine ueeine 4096 Sep  3 00:04 ..
drwxr-xr-x 2 ueeine ueeine 4096 Sep  3 00:12 unit1
```

### Q22 — Confirm with ls -la that the extraction worked. What did you find inside?

**Answer:** Yes — extraction worked, recreating the `unit1/` folder with `intro.txt` inside (confirmed by the `inflating: unit1/intro.txt` message and the `unit1` directory appearing in `ls -la`).

Command:
```
$ cd ~/cyber-course/
$ tar -czvf unit2.tar.gz unit2/
$ tar -tvf unit2.tar.gz
```

Output:
```
drwxr-xr-x ueeine/ueeine     0 2026-09-02 21:38 unit2/
-rw-r--r-- ueeine/ueeine     0 2026-09-02 21:38 unit2/b.txt
```

### Q23 — What do the flags c, z, v, and f each mean?

**Answer:** `c` creates a new archive, `z` runs it through gzip compression, `v` makes it verbose (listing each file as it's processed), and `f` tells tar the very next argument is the archive filename to use.

---

## Part 8 — Permissions

Command:
```
$ rm ~/cyber-course/hello.sh
$ touch ~/cyber-course/hello.sh
$ ls -l ~/cyber-course/hello.sh
```

Output:
```
-rw-r--r-- 1 ueeine ueeine 0 Sep  3 00:13 /home/ueeine/cyber-course/hello.sh
```

### Q24 — Paste the permission string. Can the owner execute the file?

**Answer:** `-rw-r--r--` — the owner can read and write, but there's no `x`, so execution isn't possible yet.

Content added via nano:
```
#!/bin/bash
echo "Hello from my first script"
```

Command:
```
$ ~/cyber-course/hello.sh
```

Output:
```
-bash: /home/ueeine/cyber-course/hello.sh: Permission denied
```

### Q25 — What happened, and why?

**Answer:** It failed with "Permission denied." The script content was correct (shebang and all), but without the execute bit set, the shell won't treat it as a runnable program no matter what's inside.

Command:
```
$ chmod u+x ~/cyber-course/hello.sh
$ ls -l ~/cyber-course/hello.sh
$ ~/cyber-course/hello.sh
```

Output:
```
-rwxr--r-- 1 ueeine ueeine 48 Sep  3 00:13 /home/ueeine/cyber-course/hello.sh
Hello from my first script
```

### Q26 — What does the new permission string look like? Did the script run this time?

**Answer:** `-rwxr--r--` — the `x` now appears in the owner slot. Yes, it ran this time and printed "Hello from my first script."

Command:
```
$ chmod 700 ~/cyber-course/hello.sh
$ ls -l ~/cyber-course/hello.sh
```

Output:
```
-rwx------ 1 ueeine ueeine 48 Sep  3 00:13 /home/ueeine/cyber-course/hello.sh
```

### Q27 — What does 700 mean in plain language?

**Answer:** The owner gets full access — read, write, execute (the 7). Group and everyone else get nothing at all (the two 0s). It's mine alone to open, edit, or run.

---

## Part 9 — Processes and system info

### Q28 — What does the USER column show?

Command:
```
$ ps aux | head -n 10
```

Output:
```
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0  23712 14628 ?        Ss   Sep02   0:00 /sbin/init
root           2  0.0  0.0   3180  2204 hvc0     Sl+  Sep02   0:00 /init
root           8  0.0  0.0   3180  2044 hvc0     Sl+  Sep02   0:00 plan9 --control-socket 7 --log-level 4 --server-fd 8 --pipe-fd 10 --log-truncate
root          38  0.0  0.0  50968 16140 ?        Ss   Sep02   0:00 /usr/lib/systemd/systemd-journald
root          81  0.0  0.0  33700 10068 ?        Ss   Sep02   0:00 /usr/lib/systemd/systemd-udevd
root         129  0.0  0.0   6864  2836 ?        Ss   Sep02   0:00 /usr/sbin/cron -f
message+     130  0.0  0.0   7896  4424 ?        Ss   Sep02   0:00 /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activation --syslog-only
root         142  0.0  0.0  18684  9052 ?        Ss   Sep02   0:00 /usr/lib/systemd/systemd-logind
root         175  0.0  0.0   8168  2720 tty1     Ss+  Sep02   0:00 /sbin/agetty -o -- \u --noreset --noclear - linux
```

**Answer:** Whoever started or owns that process — mostly `root` for system-level services (init, systemd-journald, cron), with `message+` for the dbus daemon (a dedicated service account).

Also ran `top`, sorted by memory (M) and CPU (P), then quit with `q`.

### Q29 — How much disk space is your cyber-course directory using?

Command:
```
$ df -h
$ du -sh ~/cyber-course/
```

Output:
```
80K     /home/ueeine/cyber-course/
```

**Answer:** 80K.

### Q30 — How much RAM does your VM have, and how much is currently used?

Command:
```
$ free -h
```

Output:
```
               total        used        free      shared  buff/cache   available
Mem:            15Gi       541Mi        15Gi       3.4Mi       179Mi        14Gi
Swap:          4.0Gi          0B       4.0Gi
```

**Answer:** 15Gi total RAM, with 541Mi currently used.

---

## Part 10 — Networking and downloads

### Q31 — What is your VM's IP address on the primary interface?

Command:
```
$ ip a
```

Output (excerpt):
```
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:15:5d:76:67:2d brd ff:ff:ff:ff:ff:ff
    inet 172.31.196.154/20 brd 172.31.207.255 scope global eth0
```

**Answer:** 172.31.196.154 on `eth0` (WSL2's internal virtual network interface — a private, non-externally-routable address).

### Q32 — Did both succeed? If one failed, what is the most likely reason?

Command:
```
$ ping -c 4 1.1.1.1
$ ping -c 4 example.com
```

Output:
```
PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=57 time=17.7 ms
64 bytes from 1.1.1.1: icmp_seq=2 ttl=57 time=32.7 ms
64 bytes from 1.1.1.1: icmp_seq=3 ttl=57 time=19.6 ms
64 bytes from 1.1.1.1: icmp_seq=4 ttl=57 time=29.8 ms
--- 1.1.1.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3006ms

PING example.com (172.66.147.243) 56(84) bytes of data.
64 bytes from 172.66.147.243: icmp_seq=1 ttl=57 time=27.8 ms
64 bytes from 172.66.147.243: icmp_seq=2 ttl=57 time=30.4 ms
64 bytes from 172.66.147.243: icmp_seq=3 ttl=57 time=26.9 ms
64 bytes from 172.66.147.243: icmp_seq=4 ttl=57 time=21.7 ms
--- example.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
```

**Answer:** Yes, both succeeded — 4/4 packets received, 0% loss for each. Since `example.com` resolved to an IP address and responded, both connectivity and DNS resolution are working correctly.

Downloaded and compared files:
```
$ wget https://www.debian.org/index.html -O ~/cyber-course/debian.html
$ less ~/cyber-course/debian.html
$ curl https://www.debian.org/ -o ~/cyber-course/debian2.html
$ diff ~/cyber-course/debian.html ~/cyber-course/debian2.html
```

Output:
```
(no output)
```

### Q33 — Are the two files identical?

**Answer:** Yes — `diff` produced no output at all, meaning `wget` and `curl` pulled down identical content for debian.org's index page.

---

## Part 11 — Package management and sudo

### Q34 — Did sudo ask for a password? Whose password?

Command:
```
$ sudo apt update
```

Output:
```
Hit:1 https://security.debian.org/debian-security trixie-security InRelease
Hit:2 https://deb.debian.org/debian trixie InRelease
Get:3 https://deb.debian.org/debian trixie-updates InRelease [47.3 kB]
Get:4 https://deb.debian.org/debian trixie-backports InRelease [54.0 kB]
Fetched 101 kB in 0s (246 kB/s)
11 packages can be upgraded. Run 'apt list --upgradable' to see them.
```

**Answer:** Yes — it asked for my own account's login password (not root's), since sudo temporarily elevates my existing user rather than switching to a separate root login.

### Q35 — Were any packages upgraded? Roughly how many?

Command:
```
$ apt list --upgradable
$ sudo apt upgrade
```

Output (excerpt):
```
11 packages can be upgraded.
Upgrading:
  base-files   libexpat1         libk5crypto3  libkrb5support0  libssl3t64  openssl-provider-legacy
  dhcpcd-base  libgssapi-krb5-2  libkrb5-3     liblzma5         openssl

Summary:
  Upgrading: 11, Installing: 0, Removing: 0, Not Upgrading: 0
  Download size: 5,563 kB
```

**Answer:** Yes — 11 packages were upgraded, matching the count flagged by `apt list --upgradable` beforehand.

### Q36 — What's one thing htop shows you that top did not?

Command:
```
$ sudo apt install htop
$ which htop
$ apt show htop | head -n 20
```

Output:
```
/usr/bin/htop

Package: htop
Version: 3.4.1-5
Priority: optional
Section: utils
Maintainer: Daniel Lange <DLange@debian.org>
Installed-Size: 434 kB
Depends: libc6 (>= 2.38), libncursesw6 (>= 6), libtinfo6 (>= 6)
Suggests: lm-sensors, lsof, strace
Homepage: https://htop.dev/
Description: interactive processes viewer
 Htop is an ncursed-based process viewer similar to top, but it
 allows one to scroll the list vertically and horizontally to see
 all processes and their full command lines.
```

**Answer:** htop is a lot more visual — color-coded per-core CPU bars, a scrollable/sortable process list controlled with arrow keys instead of typed commands, and the ability to kill or renice a process directly without hunting down its PID first.

### Q37 — What is nmap, according to the description?

Command:
```
$ apt search nmap
```

Output (excerpt):
```
nmap/stable 7.95+dfsg-3 amd64
  The Network Mapper
```

**Answer:** Per `apt search`, nmap is described as "The Network Mapper" — a network exploration and security-scanning tool used to probe hosts and discover open ports and running services.

---

## Part 12 — Putting it together

### Q38 — Paste the commands you used.

Command:
```
$ mkdir ~/report
$ hostname > ~/report/system-info.txt
$ whoami >> ~/report/system-info.txt
$ uname -a >> ~/report/system-info.txt
$ df -h >> ~/report/system-info.txt
$ date >> ~/report/system-info.txt
$ cd ~
$ zip -r report.zip report/
$ unzip -l report.zip
```

Output:
```
  adding: report/ (stored 0%)
  adding: report/system-info.txt (deflated 63%)
Archive:  report.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
        0  2026-09-02 23:47   report/
     1271  2026-09-02 23:47   report/system-info.txt
---------                     -------
     1271                     2 files
```

**Answer:** `hostname`, `whoami`, `uname -a`, `df -h`, and `date` each wrote into `~/report/system-info.txt`. A single `>` was used on the first command (`hostname`) so the file started clean, then `>>` on every command after that appended new output without erasing what came before. `zip -r` packaged the whole `report/` directory recursively, and `unzip -l` confirmed the archive's contents without extracting anything.

---

## Reflection

Going through this, `less` was the command that surprised me most — I'd always just reached for `cat` on everything, but once a file gets long that's clearly the wrong tool, and being able to search inside it with `/` makes it way more useful than scrolling blind. The command I'll probably lean on most day to day is `grep`, since searching through logs or config files by hand doesn't scale once there's more than a screenful of text. The thing that's still a little fuzzy for me is exactly how permission bits interact with directories versus files — I get `chmod 700` on a script, but I'm less confident reasoning about what execute permission actually means on a folder. I also ran into a real-world case of this: this Debian install runs under WSL2 rather than a standalone VM, so it uses systemd's journal instead of a flat `/var/log/syslog` file — I had to substitute `journalctl` for the syslog-based commands in Parts 4 and 5, which was a good reminder that command output can vary meaningfully across otherwise-similar systems. Tying this back to security: this is basically the toolkit you'd use to investigate a machine after something's gone wrong — `grep`/`find` to hunt through logs for suspicious activity, `ps`/`top` to spot a process that shouldn't be running, and permissions to understand (or lock down) who can touch what in the first place.
