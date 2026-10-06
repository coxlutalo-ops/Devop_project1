# Devops notes(Module 1.2-Files operation)
The problem this solves
Your first task on any new server: make a backup of a config file before you edit it. Your second task: find it again after you moved it. Your third task: clean up the scratch files you made. File operations are not glamorous. They are what you do every single day.
# 1.Creating files and directories

#### Make a single directory
>mkdir dev

#### Make multiple directories at once
>mkdir ops bakupdir

#### Make a directory and all parents at once (no error if it already exists)
>mkdir -p /opt/dev/ops/devops/test

#### Create an empty file
>touch testfile1.txt

#### Create many files at once using brace expansion
>touch devopsfile{1..10}.txt

>ls

# 2. Copying files

#### Copy a file to a directory
>cp devopsfile1.txt dev/

>ls dev/

#### Copy using an absolute destination path
>cp /home/vagrant/devopsfile2.txt /home/vagrant/dev/

#### Copy a directory (requires -r for recursive)
>cp -r dev bakupdir/

>ls bakupdir/

# 3.  Moving and renaming

### Move a file into a directory
>mv devopsfile3.txt ops/

### Move (rename) an entire directory
>mv ops dev/

### Rename a file in place
> mv testfile1.txt testfile22.txt

### Move all .txt files into a directory
>ls

>mkdir textdir

>mv *.txt textdir/

>ls textdir/

# 4. Deleting

#### Delete a single file
>rm devopsfile10.txt

#### Delete an empty directory
>mkdir mobile

>rm mobile          # Will fail — use rmdir or -r

#### Delete a directory and everything inside
>rm -r mobile

#### Force-delete everything (no prompts)
>rm -rf *

# ⚠️ BLAST RADIUS — rm -rf

rm -rf has no undo. No trash bin. No recovery (without backups). Real engineers
have deleted production data with one misplaced space.
## Safe habits:
- Always ls the target first — confirm what's there before deleting
- Never use rm -rf with an unquoted variable — if $DIR is unset, this expands to rm -rf "/".
- In CI/CD scripts, prefer find ... -delete with explicit filters

#### Example of the dangerous space — a single space after / deletes root:
>rm -rf / tmp/foo   # CATASTROPHIC. Always verify the path before running rm -rf.

# 5. Reading files

#### Print entire file to screen
>cat /etc/hostname
>cat /etc/os-release

#### Page through a long file (q to quit, /term to search, n for next match)
>less /etc/passwd

#### First 10 lines (default)
>head /etc/passwd

#### First 20 lines
>head -20 /etc/passwd

#### Last 10 lines
>tail /etc/passwd

#### Last 2 lines
>tail -2 /etc/passwd

### Follow a file as it grows — the most-used log debugging command


>tail -f /var/log/syslog    # Ubuntu
#### On RHEL/CentOS use: tail -f /var/log/messages
#### On systemd systems (Ubuntu 16+) , prefer: journalctl -f
#### Ctrl+C to stop

# Symlinks

A symlink is an alias — a pointer to another file or directory. The original can be anywhere; the symlink is how you refer to it.

## Create a symlink 

>ln -s  /opt/dev/ops/devops/test/commands.txt cmds

>ls -l    # cmds -> /opt/dev/ops/devops/test/commands.txt

>cat cmds   # Reads from the target

* Remove a symlink (does not delete the original)
   unlink cmds

Checkpoint
After this module:

* Create, copy, move, and delete files and directories confidently

*  Understand when cp -r vs cp is needed

* Read a file 5 ways (cat, less, head, tail, tail -f)
* Explain the blast radius of rm -rf to a classmate

