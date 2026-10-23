# Module 1.5 — Pipes and Redirection


`The problem this solves
You want to: save a system health snapshot to a file, filter 10,000 log lines down to the 3 that matter, count how many processes are running, and capture errors separately from normal output. Pipes and redirection make all of this one-liners.`
---
Standard streams — what every command has
Every Linux command deals with three streams:

|   Stream | Number |Default destination            |
|----------|--------|-------------------------------|
|`stdin` (0)|Input  |Keyboard                       |
|`stdout` (1)|Normal output| Terminal screen        |
|`stderr` (2)|Error output | Terminal screen        |

### Redirection and pipes control where these streams go.
---
Redirection — saving output to files

### Write stdout to a file (OVERWRITES if file exists)
>uptime > /tmp/sysinfo.txt

>cat /tmp/sysinfo.txt

#### Overwrite again to see the difference
>ls > /tmp/sysinfo.txt

>cat /tmp/sysinfo.txt

#### APPEND stdout to a file (does not overwrite)
>uptime >> /tmp/sysinfo.txt

>cat /tmp/sysinfo.txt

⚠️ BLAST RADIUS — `>` vs` >>` > > Using > when you meant `>>` silently destroys everything in the target file. > > Classic failure mode: a cron job runs `script.sh > /var/log/myscript.log` on every > cycle. Every cycle it overwrites the previous cycle's log. By the time you need to > debug an incident, the logs are gone. > > Almost always when writing from a script, you want `>>` (append), not `>` (overwrite). `>` Use `>` deliberately, and know that it destroys what was there.

Building a system health snapshot
This exact pattern comes from the DDC Linux labs. The instructor builds it step by step:

## Build the sysinfo file piece by piece
>echo "########################################" > /tmp/sysinfo.txt

>date >> /tmp/sysinfo.txt

>echo "########################################" >> /tmp/sysinfo.txt

>uptime >> /tmp/sysinfo.txt

>echo "########################################" >> /tmp/sysinfo.txt

>free -m >> /tmp/sysinfo.txt

>echo "########################################" >> /tmp/sysinfo.txt

>df -h >> /tmp/sysinfo.txt

>echo "########################################" >> /tmp/sysinfo.txt


>cat /tmp/sysinfo.txt

### That is your system inventory script in one-liner form. You will formalise it in the lab.
---
/dev/null — the black hole

#### Suppress all output (useful in scripts when you want no noise)
>free -m > /dev/null

#### Clear a file without deleting it
>cat /dev/null > /tmp/sysinfo.txt

>cat /tmp/sysinfo.txt     # Empty now

## Stderr redirection

#### A command that doesn't exist — produces an error
>freeeee -m > /dev/null          # Error goes to stderr, still prints to screen

#### Capture stderr separately
>freeeee -m 2>> /tmp/error.log

>cat /tmp/error.log

#### Capture stdout to one place, stderr to another
>free -m 1>> /tmp/sysinfo.txt

>freeeee -m 2>> /tmp/error.log

#### Capture both stdout and stderr to the same file
>free -m &>> /tmp/combined.log

>freeeee -m &>> /tmp/combined.log

>cat /tmp/combined.log

## Pipes — chaining commands
The pipe | sends the stdout of one command to the stdin of the next. The output never touches a file; it flows directly through.

### Count lines in /etc/passwd
>wc -l /etc/passwd

>wc -l < /etc/passwd    # Alternative: redirect stdin from file

### Count files in /etc
>ls /etc | wc -l

### Find only hostnames in /etc listing
>ls /etc | grep host

### Monitor logs and filter for a pattern (Ubuntu uses syslog; RHEL uses messages)
>tail /var/log/syslog | grep -i vagrant

>tail -20 /var/log/syslog | grep -i vagrant
### On RHEL/CentOS: tail /var/log/messages | grep -i vagrant

### Filter memory info from free output
>free -m | grep Mem

### Last 10 lines of a long listing
>ls -l | tail

### First 10 lines of a long listing
>ls -l | head

---

## Common pipe patterns you will use daily

### Find a running process by name
>ps aux | grep nginx

### Count running processes by name
>ps aux | grep nginx | wc -l

### Find a listening port
>ss -tlnp | grep 8080

### Filter AWS CLI output
>aws s3 ls | grep mybucket

### Combine multiple filters
>cat /var/log/auth.log | grep "Failed password" | grep "root" | tail -20

---
## Checkpoint
After this module:

    Explain the difference between > and >> (and the blast radius of confusion)
    Redirect stderr separately from stdout
    Chain 3 commands with pipes to answer a real question about the system
    Build the system health snapshot by hand