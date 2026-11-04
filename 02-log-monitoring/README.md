# Project 02: Log Monitoring

## Objective
Learn to read a Linux authentication log, create failed logins on purpose, and find them the way a security analyst would.

## Environment
Ubuntu 26.04 LTS in VirtualBox (SecLab-01). Log file: `/var/log/auth.log`

## What the log is
`auth.log` records logins, `sudo` use and password failures. Each line has four parts: a timestamp, the machine name, the program that wrote it, and the message.

## Steps

**1. Confirm the log exists and check its permissions**
`ls -l /var/log/auth.log` showed `-rw-r----- 1 syslog adm`. The owner is `syslog`, the group `adm` can read it, and everyone else has no access. My user is in the `adm` group, so I can read it without `sudo`.

**2. Read normal activity**
`tail -n 20 /var/log/auth.log` showed a normal boot and one successful login: system start-up, the login screen sessions, my session opening, and the login screen sessions closing.

**3. Create failed logins on purpose**
I ran `sudo ls` and entered a wrong password three times. `sudo` stopped after 3 incorrect attempts.

**4. Find the failures in the log**
- `grep "password check failed" /var/log/auth.log` listed every failed attempt.
- `grep -c "password check failed" /var/log/auth.log` counted them.
- `grep "password check failed" /var/log/auth.log | grep 2026-11-04` isolated the failures from one day by sending the first search into a second one with a pipe (`|`).

## Findings
- The log counted 8 failures in total, but only 6 were from 2026-11-04. The other 2 were my own typos on 2026-11-02, when I was first learning which password `sudo` uses.
- A `sudo` authentication failure line shows who tried (`logname=carmen`, `uid=1000`), the tool (`sudo`), and the terminal (`tty=/dev/pts/0`). `rhost=` was empty, which means the attempt came from the local machine, not the network.
- Three failures about 9 seconds apart for the same user is a small version of a brute-force pattern: many failed guesses in a short time.

## Screenshot
(3.jpeg)

## Lessons learned
- `grep -c` counts the whole file. To count one day, I needed the date filter first. I reported 8 at first, then corrected it to 6 after I checked the dates.
- Timestamps can show different time zones. The earliest entry ended in `+00:00` and the later ones in `-05:00`, so mixed time zones can confuse a timeline.
- In a real investigation, I would keep the original record and add a dated correction instead of editing it, so nothing looks tampered with.
- Knowing what normal looks like is what makes abnormal activity stand out.
- A missing leading / makes the path wrong. 

## Self-test
I also completed a practice "shift report": I recorded tool versions, the day's failed-login count, the `auth.log` owner and group, and the permissions on my own report file.

## Next
Project 03: a Python script that counts failed logins automatically.
