## Part 1 - Exploring `~/.bashrc`

### Q1

```
$ ls -la ~ | grep bashrc
-rw-r--r-- 1 ueeine ueeine 3526 Sep  2 21:20 .bashrc
```

The file is 3526 bytes. Last modified Sep 2 at 21:20.

### Q2

```bash
# don't put duplicate lines or lines starting with space in the history.
# See bash(1) for more options
HISTCONTROL=ignoreboth

# append to the history file, don't overwrite it
shopt -s histappend
```

This part controls how bash saves your command history — it skips duplicate lines and adds new commands to the end of the history file instead of wiping it out each time.

### Q3

```bash
# some more ls aliases
#alias ll='ls -l'
#alias la='ls -A'
#alias l='ls -CF'
```

These are already in the default `.bashrc` but they're commented out, so they're not actually active unless you remove the `#`. The two named in the question are `ll` and `la`.

## Part 2 - Backup Before Editing

### Q4

```
$ cp ~/.bashrc ~/.bashrc.backup
$ ls -la ~/.bashrc*
-rw-r--r-- 1 ueeine ueeine 3526 Sep  2 21:20 /home/ueeine/.bashrc
-rw-r--r-- 1 ueeine ueeine 3526 Sep 13 01:11 /home/ueeine/.bashrc.backup
```

Both files are there, same size, backup has today's timestamp.

## Part 3 - Adding a Welcome Banner

### Q5

Added to the bottom of `.bashrc`:
```bash
# My customizations start here
echo "Hello, dolma"
```

Opened a new terminal and got:
```
Hello, dolma
```

### Q6

Replaced it with the personalized version:
```bash
echo "==============================="
echo "  Welcome back, $(whoami)"
echo "  Host: $(hostname)"
echo "  Today: $(date '+%A, %d %B %Y')"
echo "==============================="
```

Output in a new terminal:
```
===============================
  Welcome back, ueeine
  Host: dolma
  Today: Sunday, 13 September 2026
===============================
```

### Q7

`$(whoami)` runs the `whoami` command and drops its output right into the text. The `$()` part is command substitution — it tells bash "run this command first, then put whatever it printed here."

## Part 4 - Adding Aliases

### Q8

Added:
```bash
alias ll='ls -la'
alias gohome='cd ~/cyber-course'
```

After running `source ~/.bashrc`:
```
$ ll
total 68
drwx------ 7 ueeine ueeine 4096 Sep 13 01:19 .
drwxr-xr-x 3 root   root   4096 Sep  2 21:20 ..
-rw------- 1 ueeine ueeine 6999 Sep  7 00:17 .bash_history
-rw-r--r-- 1 ueeine ueeine  220 Sep  2 21:20 .bash_logout
-rw-r--r-- 1 ueeine ueeine 3757 Sep 13 01:19 .bashrc
-rw-r--r-- 1 ueeine ueeine 3526 Sep 13 01:11 .bashrc.backup
drwx------ 3 ueeine ueeine 4096 Sep  2 23:32 .config
drwxr-xr-x 6 ueeine ueeine 4096 Sep  3 00:13 cyber-course
...

$ gohome
$ pwd
/home/ueeine/cyber-course
```

### Q9

```
$ alias
alias gohome='cd ~/cyber-course'
alias ll='ls -la'
alias ls='ls --color=auto'
```

3 aliases total: my two new ones plus `ls --color=auto`, which is the one Debian default that's actually turned on (the `ll`/`la`/`l` examples in the file are commented out, so they don't count).

### Q10

`gohome` is the one I actually use — saves me typing out the full path every time I want to get to my course folder.

## Part 5 - History Settings

### Q11

```
$ echo $HISTSIZE
1000
$ echo $HISTFILESIZE
2000
```

### Q12

```
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

344 lines total. Mostly just me practicing basic navigation commands, nothing sensitive.

### Q13

Changed the values in `.bashrc` to:
```
HISTSIZE=10000
HISTFILESIZE=20000
```

After sourcing the file:
```
$ echo $HISTSIZE
10000
$ echo $HISTFILESIZE
20000
```

### Q14

```
$ HISTSIZE=5
$ history
  361  source ~/.bashrc
  362  echo $HISTSIZE
  363  echo $HISTFILESIZE
  364  HISTSIZE=5
  365  history
```

Only shows the last 5 commands now, even though the numbering keeps going from before. This only changes the current terminal — the `.bashrc` file still says 10000, so a new terminal resets it back.

### Q15

If someone got into my home folder and read `.bash_history`, they could see passwords or tokens I might've typed by accident, plus usernames, file paths, and whatever commands I've been running — basically a trail of what I've been doing on the system.

## Part 6 and 7 - First Script and Testing

Wrote the script in `~/cyber-course/make-files.sh`:

```bash
#!/bin/bash
# make-files.sh — Ask for a directory name, create it if needed,
#                 and populate it with 5 empty files.
# Author: ueeine
# Date:   Sep 13 2026

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

### Q16 - New directory

```
$ ./make-files.sh
Enter a directory name: test-run-1
Created directory: test-run-1
Created 5 files in test-run-1

$ ls -la test-run-1/
total 8
drwxr-xr-x 2 ueeine ueeine 4096 Sep 13 01:27 .
drwxr-xr-x 7 ueeine ueeine 4096 Sep 13 01:27 ..
-rw-r--r-- 1 ueeine ueeine    0 Sep 13 01:27 file1.txt
-rw-r--r-- 1 ueeine ueeine    0 Sep 13 01:27 file2.txt
-rw-r--r-- 1 ueeine ueeine    0 Sep 13 01:27 file3.txt
-rw-r--r-- 1 ueeine ueeine    0 Sep 13 01:27 file4.txt
-rw-r--r-- 1 ueeine ueeine    0 Sep 13 01:27 file5.txt
```

### Q17 - Existing directory

Ran it again with the same name:

```
$ ./make-files.sh
Enter a directory name: test-run-1
Directory already exists: test-run-1
Created 5 files in test-run-1
```

It still ran the file-creation loop even though the directory already existed. Checking the timestamps before and after proves it:

- First run: all 5 files timestamped `01:27`
- Second run: all 5 files timestamped `01:28`

So `touch` doesn't delete or overwrite the files — it just updates the modified time. The files stayed at 0 bytes both times because they were already empty, but if they'd had real content in them, that content would've been left alone. The `if [ -d "$dirname" ]` check only decides which message gets printed, it doesn't actually stop the loop from running again.

### Q18 - Empty input

```
$ ./make-files.sh
Enter a directory name:
Error: no name was given.

$ echo $?
1
```

Exits with status 1, no directory or files get created.

## Part 8 - Improvement

I went with **Option C — refuse to overwrite**. If the directory already exists and already has stuff in it, the script bails out instead of touching anything.

```
$ ./make-files.sh
Enter a directory name: safe-test
Created directory: safe-test
Created 5 files in safe-test

$ ./make-files.sh
Enter a directory name: safe-test
Error: directory already exists and is not empty: safe-test
```

Second run correctly refuses — this fixes the exact issue from Q17.

## script

See `make-files.sh` in this folder. Full contents:

```bash
#!/bin/bash
# make-files.sh — Ask for a directory name, create it if needed,
#                 and populate it with 5 empty files.
# Improvement (Option C): refuses to touch a non-empty existing directory.

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
```
