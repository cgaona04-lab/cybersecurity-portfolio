# Project 01: Tools Setup
# Linux Basics Practice

Environment: Ubuntu 26.04 LTS in VirtualBox (SecLab-01)

## Goal
Practice the terminal commands I will use every day as a security analyst, using a simple "Day 1 analyst work log" as the exercise.

## Commands I practiced

| Command | What it does |
|---|---|
| `pwd` | Shows the folder I am in |
| `ls -a` | Lists files, including hidden ones |
| `mkdir lab` | Creates a folder called `lab` |
| `cd lab` | Moves into the folder |
| `touch notes.txt` | Creates an empty file |
| `echo "text" > notes.txt` | Writes text to a file (overwrites it) |
| `echo "text" >> notes.txt` | Adds a line to the end of a file |
| `cat notes.txt` | Displays the file |
| `grep failed notes.txt` | Searches the file for a word |
| `ls -l` | Shows permissions, owner, and size |

## Exercise: Day 1 work log
I built a work log with four tasks an entry-level analyst might do:

- Day 1: Review overnight security alerts
- Check failed login attempts in auth.log
- Verify system updates are installed
- Document anything suspicious

I then used `grep failed notes.txt` to find the line about failed logins, the same way an analyst searches a log for a keyword.

## Reading file permissions
`ls -l` showed `-rw-rw-r--` for `notes.txt`:

- `-` : regular file
- `rw-` : owner can read and write
- `rw-` : group can read and write
- `r--` : everyone else can only read

`r` = read, `w` = write, `x` = execute. For a sensitive file, "everyone else can read" would be a risk, so analysts check permissions.

## Lessons learned
- `>` overwrites a file, `>>` adds to it. Using `>` by mistake erased my earlier lines.
- A missing closing quote leaves the terminal waiting for input. `Ctrl+C` cancels it.
- The up arrow brings back the last command so I can fix a typo instead of retyping.
- Commands are case-sensitive, so `pwd` must be lowercase.

## Next steps
Log monitoring: reading `/var/log/auth.log` and finding failed logins.
