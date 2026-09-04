# Linux CLI Basics

## Part 1

## Q1 - What username are you logged in as?

Command:
```
$ whoami
```

Output:
```
ueeine
```

ueeine.

## Q2 - Are you a member of the sudo group? How can you tell from the output of id?

Command:
```
$ id
```

Output:
```
uid=1000(ueeine) gid=1000(ueeine) groups=1000(ueeine),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),100(users)
```

Yes, `27(sudo)` is in the groups list.

## Q3 - What kernel version is your system running?

Command:
```
$ uname -a
```

Output:
```
Linux dolma 6.18.33.2-microsoft-standard-WSL2 #1 SMP PREEMPT_DYNAMIC Thu Jun 18 21:54:43 UTC 2026 x86_64 GNU/Linux
```

6.18.33.2-microsoft-standard-WSL2. The "microsoft-standard-WSL2" part is what tells you it's WSL2 and not a regular VM.

## Q4 - What is the difference in the depth of information whatis and man give you?

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

what is gives you just the one-line description from the top of the man page. man gives you the whole page - all the flags, examples, related commands. I'd use whatis if I already kind of know the command, man if I need to actually learn it.

## Q5 - While in man, how do you (a) search for "user" and (b) quit?

`/user` then Enter jumps to the first match, `n` goes to the next one. `q` quits.

---

## Part 2

## Q6 - What did cd - do?

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

It sent me back to the previous directory (`/`) and printed it. It's basically a shortcut for jumping between two directories without retyping the path.

## Q7 - What additional information does -l give you over plain ls?

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

Plain ls just shows names. -l adds permissions, link count, owner, group, size, and last modified date.

## Q8 - What does -a show that wasn't visible before? Name two examples.

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

Dotfiles (hidden by default) plus `.` and `..`. In /etc that's `.pwd.lock` and `.updated`.

## Q9 - What is the largest file in /var/log? What size is it?

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

dpkg.log at 31K.

## Q10 - What was modified most recently?

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

dpkg.log again, Sep 2 23:43. -t sorts by modification time, newest first.

---

## Part 3

## Q11 - Show the command(s) you used to create the directory structure.

Command:
```
$ mkdir -p ~/cyber-course/unit1 ~/cyber-course/unit2 ~/cyber-course/unit3/{osint,recon,crypto} ~/cyber-course/scratch
```

Used brace expansion for the three unit3 subfolders so it's one command instead of three.

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

## Q12 - What key combination did you use to save? What key combination did you use to exit?

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

Ctrl+O to save (nano asks for the filename, just hit Enter), Ctrl+X to exit.

## Q13 - Why did rmdir fail (or succeed)?

Command:
```
$ rmdir ~/cyber-course/scratch
```

Output:
```
rmdir: failed to remove '/home/ueeine/cyber-course/scratch': Directory not empty
```

rmdir only works on empty directories, and scratch/ still had notes.txt in it. Used rm -r instead:
```
$ rm -r ~/cyber-course/scratch
```

---

## Part 4

## Q14 - Which Debian version do you have?

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

Debian GNU/Linux 13, codename trixie.

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

## Q15 - What kind of messages do you see? Are they recent?

Note: this system doesn't have a `/var/log/syslog` - WSL2 runs on systemd's journal instead, so /var/log only has dpkg.log, alternatives.log, and the journal/ folder. Used journalctl instead of what the assignment expected.

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

Nothing unusual - sudo sessions opening and closing, a kernel note about dropping caches, a scheduled dpkg backup. All recent, timestamps go from 23:43 up to when I ran the command.

---

## Part 5

## Q16 - How many lines were returned?

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

1.

## Search for "error" (case-insensitive) - substituted for syslog since it doesn't exist on this system

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

## Q17 - How would you modify the command to show only .conf files modified in the last 7 days?

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

Add `-mtime -7` to limit results to files modified in the last week.

## Q18 - Where are these commands actually located on the filesystem?

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

/usr/bin/ls and /usr/bin/nano.

---

## Part 6

## Q19 - What does the | symbol do here?

Command:
```
$ history | tail -n 20
```

It feeds the output of one command into the next as input, so commands can be chained instead of everything just printing to the screen.

## Q20 - What is the difference between > and >>?

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

`>` overwrites the file. `>>` appends to it. You can see that in the second cat output - the original listing is still there with the date added below it.

## Q21 - What was the output, and why?

Command:
```
$ history | grep "cd "
$ echo "hello cyber world" | grep "cyber"
```

Output:
```
hello cyber world
```

"hello cyber world" prints because echo outputs the string as-is, and it contains "cyber" so grep matches it.

## clear vs Ctrl+L

Ran `clear`, then `Ctrl+L` on a new line.

Same result on screen, different mechanism - Ctrl+L is handled by the terminal itself, clear is a separate program. Either way the scrollback isn't deleted, just scrolled out of view.

---

## Part 7

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

## Q22 - Confirm with ls -la that the extraction worked. What did you find inside?

it Worked fine - unit1/ came back with intro.txt inside it, same as it went in.

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

## Q23 - What do the flags c, z, v, and f each mean?

c = create the archive, z = compress with gzip, v = verbose (lists files as it goes), f = next argument is the archive filename.

---

## Part 8

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

## Q24 - Paste the permission string. Can the owner execute the file?

`-rw-r--r--`. No x anywhere, so no.

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

## Q25 - What happened, and why?


Permission denied. Valid shebang doesn't matter - without the execute bit the shell won't run it.

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

## Q26 - What does the new permission string look like? Did the script run this time?

`-rwxr--r--` - x showed up in the owner slot. Ran fine, printed "Hello from my first script."

Command:
```
$ chmod 700 ~/cyber-course/hello.sh
$ ls -l ~/cyber-course/hello.sh
```

Output:
```
-rwx------ 1 ueeine ueeine 48 Sep  3 00:13 /home/ueeine/cyber-course/hello.sh
```

## Q27 - What does 700 mean in plain language?

Owner gets full read/write/execute (the 7). Everyone else gets nothing.

---

## Part 9

## Q28 - What does the USER column show?

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

Whoever owns/started the process. Mostly root since these are system services, plus message+ for the dbus daemon, which runs under its own account.

Also ran top, sorted by memory and CPU, quit with q.

## Q29 - How much disk space is your cyber-course directory using?

Command:
```
$ df -h
$ du -sh ~/cyber-course/
```

Output:
```
80K     /home/ueeine/cyber-course/
```

80K total.

## Q30 - How much RAM does your VM have, and how much is currently used?

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

15Gi total, 541Mi used right now.

---

## Part 10

## Q31 - What is your VM's IP address on the primary interface?

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

172.31.196.154 on eth0 - WSL2's internal virtual adapter, not reachable from outside the machine.

## Q32 - Did both succeed? If one failed, what is the most likely reason?

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

oth succeeded, 4/4 with 0% packet loss. example.com resolving and answering means DNS is working too, not just raw connectivity.

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

## Q33 - Are the two files identical?

Yes, diff returned nothing, so wget and curl pulled the same page.

---

## Part 11

## Q34 - Did sudo ask for a password? Whose password?

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

Yes, my own password, not root's. sudo temporarily elevates my account instead of logging in as root.

## Q35 - Were any packages upgraded? Roughly how many?

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

11, matches what apt list --upgradable showed beforehand.

## Q36 - What's one thing htop shows you that top did not?

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

It's just easier to use - color-coded bars per core, scroll with arrow keys, and you can kill or renice a process directly instead of hunting for its PID first.

## Q37 - What is nmap, according to the description?

Command:
```
$ apt search nmap
```

Output (excerpt):
```
nmap/stable 7.95+dfsg-3 amd64
  The Network Mapper
```

apt search calls it "The Network Mapper" - used for probing hosts, checking open ports, and seeing what services are running.

---

## Part 12

## Q38 - Paste the commands you used.

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

hostname, whoami, uname -a, df -h, and date each get written into system-info.txt in order. The first one uses `>` so the file starts clean, the rest use `>>` so they append instead of overwriting. zip -r grabs the whole report/ folder recursively, unzip -l just lists what's in the archive without extracting.
