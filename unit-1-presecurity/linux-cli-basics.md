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

**Answer:** ueeine

### Q2 — Are you a member of the sudo group? How can you tell from the output of id?

Command:
```
$ id
```

Output:
```
uid=1000(ueeine) gid=1000(ueeine) groups=1000(ueeine),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),100(users)
```

**Answer:** Yep. `27(sudo)` shows up in the groups list so I'm in.

### Q3 — What kernel version is your system running?

Command:
```
$ uname -a
```

Output:
```
Linux dolma 6.18.33.2-microsoft-standard-WSL2 #1 SMP PREEMPT_DYNAMIC Thu Jun 18 21:54:43 UTC 2026 x86_64 GNU/Linux
```

**Answer:** 6.18.33.2-microsoft-standard-WSL2. The "microsoft-standard-WSL2" bit gives away that it's WSL2, not a regular VM.

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

**Answer:** whatis just gives you the one-liner off the top of the man page, basically a quick reminder of what a command does. man is the whole document — every flag, examples if there are any, related commands, all of it. If you already kind of know the command, whatis. If you need to actually learn it, man.

### Q5 — While in man, how do you (a) search for "user" and (b) quit?

**Answer:** `/user` then Enter jumps to the first hit, `n` goes to the next one. `q` quits out.

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

**Answer:** It jumped back to whatever directory I was in before (`/`) and printed the path. Handy for bouncing between two spots without typing the whole path again.

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

**Answer:** Regular `ls` is just names in a grid. `-l` adds permissions, number of links, owner, group, size, and when it was last modified.

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

**Answer:** Dotfiles — anything starting with a period gets hidden by default — plus `.` and `..`. In `/etc` I could see `.pwd.lock` and `.updated`, neither of which show up without `-a`.

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

**Answer:** dpkg.log at 31K, by a wide margin.

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

**Answer:** dpkg.log again, Sep 2 23:43. `-t` sorts by mod time newest-first so it floats to the top.

---

## Part 3 — Creating and managing files

### Q11 — Show the command(s) you used to create the directory structure.

Command:
```
$ mkdir -p ~/cyber-course/unit1 ~/cyber-course/unit2 ~/cyber-course/unit3/{osint,recon,crypto} ~/cyber-course/scratch
```

**Answer:** Did it in one line using brace expansion for the three unit3 subfolders instead of running mkdir three separate times.

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

**Answer:** Ctrl+O to write out (it'll ask you to confirm the filename, hit Enter), Ctrl+X to exit.

### Q13 — Why did rmdir fail (or succeed)?

Command:
```
$ rmdir ~/cyber-course/scratch
```

Output:
```
rmdir: failed to remove '/home/ueeine/cyber-course/scratch': Directory not empty
```

**Answer:** rmdir refuses to touch anything that isn't completely empty, and scratch/ still had notes.txt sitting in it. Had to use rm -r instead to actually clear it:
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

**Answer:** Debian GNU/Linux 13, trixie.

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

Note: this system doesn't have a `/var/log/syslog` at all — WSL2 uses systemd's journal instead, so `/var/log` only has `dpkg.log`, `alternatives.log`, and the `journal/` folder. Had to use `journalctl` instead of the commands the assignment expected.

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

**Answer:** Pretty mundane stuff — sudo sessions opening and closing, a kernel message about dropping caches, and a scheduled dpkg backup job. And yeah, all recent, the timestamps run from 23:43 up to basically the moment I ran the command.

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

**Answer:** 1

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
(ran with sudo since some directories like /etc/ssl/private and /etc/credstore threw Permission denied otherwise)

**Answer:** Add `-mtime -7` and it only shows files touched in the last week.

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

**Answer:** /usr/bin/ls and /usr/bin/nano.

---

## Part 6 — History, redirection, and pipes

### Q19 — What does the | symbol do here?

Command:
```
$ history | tail -n 20
```

**Answer:** Pipes the output of one command straight into the next one as input, so you can chain stuff together instead of it just printing to the screen.

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

**Answer:** `>` blows away whatever's in the file and writes fresh. `>>` tacks new output onto the end without touching what's already there — you can see that in the second cat, the original listing is still intact with the date line just added below it.

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

**Answer:** "hello cyber world" comes out because echo prints that string as-is, and it contains "cyber" so grep lets it through.

### clear vs Ctrl+L

Ran `clear`, then `Ctrl+L` on a new line.

**Answer:** Same visible result, different mechanism — Ctrl+L is handled by the terminal itself, clear is an actual separate program that does the same job. Either way your scrollback is still there, it's just pushed out of view.

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

**Answer:** Worked fine — the unit1/ folder came back with intro.txt inside it, same as it went in.

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

**Answer:** c = create, z = gzip it, v = verbose so it lists files as it goes, f = the next thing on the command line is the archive filename.

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

**Answer:** `-rw-r--r--`. No x anywhere, so no, can't execute it yet.

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

**Answer:** Permission denied. Doesn't matter that the script itself is fine with a proper shebang — without the execute bit the shell just won't run it.

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

**Answer:** `-rwxr--r--`, x is there now in the owner spot. And yes, ran fine, printed "Hello from my first script."

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

**Answer:** Owner (me) can read, write, and execute — that's the 7. Everyone else, group included, gets zero access at all.

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

**Answer:** Whoever owns/started the process. Mostly root since these are all system services, and message+ for the dbus daemon which runs under its own dedicated account.

Also ran top, sorted by memory and CPU, quit with q.

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

**Answer:** 15Gi total, only 541Mi actually used right now.

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

**Answer:** 172.31.196.154 on eth0 — that's WSL2's internal virtual adapter, not something reachable from outside.

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

**Answer:** Both worked, 4/4 with zero loss on each. example.com resolving and responding also means DNS is working, not just raw connectivity.

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

**Answer:** Yep, diff came back empty so wget and curl grabbed the exact same page.

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

**Answer:** Yeah, but my own password, not root's. sudo just temporarily elevates my account rather than logging in as a separate root user.

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

**Answer:** 11, matches what apt list --upgradable said beforehand.

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

**Answer:** Way easier to actually use — color-coded bars per core, you can scroll around with arrow keys instead of typing, and you can kill or renice something directly instead of having to go look up its PID first.

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

**Answer:** apt search just calls it "The Network Mapper" — it's for probing hosts, finding open ports, seeing what services are running.

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

**Answer:** hostname, whoami, uname -a, df -h, and date each get piped into system-info.txt. Only the first one uses a single `>` so it starts clean, then everything after uses `>>` so it keeps appending instead of wiping the file each time. zip -r grabs the whole report/ folder recursively, and unzip -l just lists what's in the archive without pulling anything out.

---

## Reflection

Honestly `less` was the one that surprised me most. I've just been using `cat` for everything, which is fine until a file actually has some length to it — then it's obviously the wrong tool, and being able to hit `/` and search inside instead of scrolling forever makes a real difference. I think `grep` is probably what I'll end up using the most going forward, since digging through logs or configs by eye stops working the second there's more than a screen's worth of text.

Still a bit fuzzy on how permission bits actually work on directories versus files. `chmod 700` on a script makes sense to me, but I couldn't confidently explain what the execute bit even does on a folder if you asked me right now.

One real hiccup: this box runs Debian under WSL2 instead of a normal VM, so there's no flat `/var/log/syslog` — it's all in systemd's journal instead. Had to swap in `journalctl` for a few of the syslog-based questions in Parts 4 and 5. Good reminder that the same OS can behave differently depending on what it's sitting on top of.

And thinking about it from a security angle — this is basically exactly what you'd reach for if you were investigating a machine after something went wrong. grep and find to dig through logs for anything suspicious, ps and top to catch a process that shouldn't be there, and permissions to figure out (or lock down) who's actually allowed to touch what.
