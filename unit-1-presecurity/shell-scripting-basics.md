## Part 1 — Exploring `~/.bashrc`
 
**Q1 — File info**
 
```bash
$ ls -la ~ | grep bashrc
-rw-r--r-- 1 ueeine ueeine 3526 Sep  2 21:20 .bashrc
```
 
3526 bytes, last modified Sep 2 at 21:20.
 
**Q2 — A commented section**
 
```bash
# don't put duplicate lines or lines starting with space in the history.
HISTCONTROL=ignoreboth
 
# append to the history file, don't overwrite it
shopt -s histappend
```
 
> Controls how bash saves command history — skips duplicates, appends instead of overwriting.
 
**Q3 — Default aliases**
 
```bash
# some more ls aliases
#alias ll='ls -l'
#alias la='ls -A'
#alias l='ls -CF'
```
 
> Commented out by default, so not actually active. The two named: `ll` and `la`.
 
---
 
## Part 2 — Backup Before Editing
 
**Q4**
 
```bash
$ cp ~/.bashrc ~/.bashrc.backup
$ ls -la ~/.bashrc*
-rw-r--r-- 1 ueeine ueeine 3526 Sep  2 21:20 /home/ueeine/.bashrc
-rw-r--r-- 1 ueeine ueeine 3526 Sep 13 01:11 /home/ueeine/.bashrc.backup
```
 
Both files exist, same size, backup timestamped today.
 
---
 
## Part 3 — Welcome Banner
 
**Q5 — First test**
 
```bash
# My customizations start here
echo "Hello, dolma"
```
 
```text
Hello, dolma
```
 
**Q6 — Personalized banner**
 
```bash
echo "==============================="
echo "  Welcome back, $(whoami)"
echo "  Host: $(hostname)"
echo "  Today: $(date '+%A, %d %B %Y')"
echo "==============================="
```
 
```text
===============================
  Welcome back, ueeine
  Host: dolma
  Today: Sunday, 13 September 2026
===============================
```
 
**Q7 — What does `$(whoami)` do?**
 
> Runs `whoami` and drops its output into the surrounding text. `$()` is command substitution — run the command first, then insert what it printed.
 
---
 
## Part 4 — Aliases
 
**Q8**
 
```bash
alias ll='ls -la'
alias gohome='cd ~/cyber-course'
```
 
```bash
$ ll
total 68
drwx------ 7 ueeine ueeine 4096 Sep 13 01:19 .
drwxr-xr-x 3 root   root   4096 Sep  2 21:20 ..
-rw------- 1 ueeine ueeine 6999 Sep  7 00:17 .bash_history
-rw-r--r-- 1 ueeine ueeine 3757 Sep 13 01:19 .bashrc
drwxr-xr-x 6 ueeine ueeine 4096 Sep  3 00:13 cyber-course
...
 
$ gohome
$ pwd
/home/ueeine/cyber-course
```
 
**Q9 — Alias count**
 
```bash
$ alias
alias gohome='cd ~/cyber-course'
alias ll='ls -la'
alias ls='ls --color=auto'
```
 
> 3 total: my 2 new ones + `ls --color=auto`, the only Debian default that's actually turned on.
 
**Q10 — Reflection**
 
> `gohome` is the one I actually use — saves typing the full path every time.
 
---
 
## Part 5 — History Settings
 
**Q11 — Defaults**
 
```bash
$ echo $HISTSIZE
1000
$ echo $HISTFILESIZE
2000
```
 
**Q12 — History file**
 
```bash
$ wc -l ~/.bash_history
344 /home/ueeine/.bash_history
 
$ tail -n 20 ~/.bash_history
...
cd -
whoami
cd ~
pwd
cd /
pwd
```
 
**Q13 — New limits**
 
```bash
HISTSIZE=10000
HISTFILESIZE=20000
```
 
```bash
$ echo $HISTSIZE
10000
$ echo $HISTFILESIZE
20000
```
 
**Q14 — Temporary override**
 
```bash
$ HISTSIZE=5
$ history
  361  source ~/.bashrc
  362  echo $HISTSIZE
  363  echo $HISTFILESIZE
  364  HISTSIZE=5
  365  history
```
 
> Only shows the last 5 commands, but only in this shell — `.bashrc` still says 10000, so a new terminal resets it.
 
**Q15 — Security thought**
 
> Anyone reading `.bash_history` could find passwords or tokens typed by accident, plus usernames, file paths, and a trail of what I've been doing on the system.
 
---
 
## Parts 6–7 — First Script & Testing
 
Base script, written to `~/cyber-course/make-files.sh`:
 
```bash
#!/bin/bash
 
read -p "Enter a directory name: " dirname
 
if [ -z "$dirname" ]; then
    echo "Error: no name was given."
    exit 1
fi
 
if [ -d "$dirname" ]; then
    echo "Directory already exists: $dirname"
else
    mkdir "$dirname"
    echo "Created directory: $dirname"
fi
 
for i in {1..5}; do
    touch "$dirname/file${i}.txt"
done
 
echo "Created 5 files in $dirname"
```
 
**Q16 — New directory**
 
```bash
$ ./make-files.sh
Enter a directory name: test-run-1
Created directory: test-run-1
Created 5 files in test-run-1
 
$ ls -la test-run-1/
total 8
-rw-r--r-- 1 ueeine ueeine 0 Sep 13 01:27 file1.txt
-rw-r--r-- 1 ueeine ueeine 0 Sep 13 01:27 file2.txt
-rw-r--r-- 1 ueeine ueeine 0 Sep 13 01:27 file3.txt
-rw-r--r-- 1 ueeine ueeine 0 Sep 13 01:27 file4.txt
-rw-r--r-- 1 ueeine ueeine 0 Sep 13 01:27 file5.txt
```
 
**Q17 — Existing directory**
 
```bash
$ ./make-files.sh
Enter a directory name: test-run-1
Directory already exists: test-run-1
Created 5 files in test-run-1
```
 
Timestamps before → after:
 
| File | Run 1 | Run 2 |
|---|---|---|
| file1.txt | 01:27 | 01:28 |
| file2.txt | 01:27 | 01:28 |
| file3.txt | 01:27 | 01:28 |
 
> The loop still ran on the second pass. `touch` doesn't delete or overwrite content — it just bumps the modified time. Files stayed 0 bytes since they were already empty, but real content would've survived untouched. The `if [ -d "$dirname" ]` check only controls the message, not whether the loop runs.
 
**Q18 — Empty input**
 
```bash
$ ./make-files.sh
Enter a directory name:
Error: no name was given.
 
$ echo $?
1
```
 
Exits with status `1`, nothing created.
 
---
 
## Part 8 — Improvement
 
**Option C — refuse to overwrite.** If the directory exists and already has files in it, the script exits instead of touching anything.
 
```bash
$ ./make-files.sh
Enter a directory name: safe-test
Created directory: safe-test
Created 5 files in safe-test
 
$ ./make-files.sh
Enter a directory name: safe-test
Error: directory already exists and is not empty: safe-test
```
 
Second run correctly refuses — fixes the exact gap found in Q17.
 
---
 
## Final Script
 
Full script lives in [`make-files.sh`](./make-files.sh):
 
```bash
#!/bin/bash
 
read -p "Enter a directory name: " dirname
 
if [ -z "$dirname" ]; then
    echo "Error: no name was given."
    exit 1
fi
 
if [ -d "$dirname" ] && [ "$(ls -A "$dirname")" ]; then
    echo "Error: directory already exists and is not empty: $dirname"
    exit 1
fi
 
if [ -d "$dirname" ]; then
    echo "Directory already exists and is empty: $dirname"
else
    mkdir "$dirname"
    echo "Created directory: $dirname"
fi
 
for i in {1..5}; do
    touch "$dirname/file${i}.txt"
done
 
echo "Created 5 files in $dirname"
