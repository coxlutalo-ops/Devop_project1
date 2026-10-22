 #  Module 1.4 — Permissions


 The problem this solves 

A Jenkins CI/CD job fails with "Permission denied". You SSH into the server. You see a file. How do you know who owns it, who can read it, and what exactly is blocked? Permissions tell you. This module teaches you to read and fix them.
Reading ls -l output

 `ls -l /opt/devopsdir      `

Example output:

` drwxrwx--- 2 ansible devops 6 Dec  7 10:30 /opt/devopsdir `

Breaking it down:

d  rwx  rwx  ---   2  ansible  devops  6  Dec 7  /opt/devopsdir
│   │    │    │
│   │    │    └── Other:  no permissions
│   │    └─────── Group:  read, write, execute
│   └──────────── Owner:  read, write, execute
└──────────────── Type:   d=directory, -=file, l=symlink

The 9 permission characters = 3 groups × 3 bits (r=read, w=write, x=execute):

    For files: x means the file can be run as a program
    For directories: x means you can cd into it; without x, the directory is sealed

Users and groups — the who

### Check who you are and what groups you belong to
> whoami
 
 >id 

### Create users
>sudo useradd ansible

>sudo useradd jenkins

>sudo useradd aws

### Verify
>id ansible

>tail -4 /etc/passwd

>tail -4 /etc/group

### Create a group
>sudo groupadd devops

### Add a user to a group
>sudo usermod -aG devops ansible

>id ansible           # Should show devops in the groups list

>grep devops /etc/group

 ## Changing ownership — chown

### Create a shared directory
>sudo mkdir /opt/devopsdir

>sudo mkdir /opt/webdata

### Set owner and group
>sudo chown ansible:devops /opt/devopsdir

>ls -ld /opt/devopsdir

### Short form (owner.group also works)
>sudo chown aws.devops /opt/webdata

>ls -ld /opt/webdata

## Changing permissions — chmod
### Symbolic mode (reads like English — learn this first)

### Remove execute from others
>chmod o-x /opt/devopsdir
>ls -ld /opt/devopsdir

### Remove read from others
>chmod o-r /opt/devopsdir

### Add write for group
>chmod g+w /opt/devopsdir

### Add execute for owner
>chmod u+x script.sh

### Make a script executable by everyone
>chmod +x script.sh

Symbols: u=owner, g=group, o=others, a=all; +=add, -=remove, ==set exactly

### Octal mode (you will see this in docs and scripts)

Each permission bit has a numeric value: r=4, w=2, x=1. Add them per group.

| Octal         | Symoblic             | Meaning                                  |
|---------------|----------------------|------------------------------------------|
|`755`          | `rwxr-xr-x`          | Owner: all; group+others: read+execute (standard for dirs, executables)
|`644`          |  `rw-r--r--`         | Owner: read+write; group+others: read     only (standard for files)
|`600`          | `rw-------`          | Owner only; nobody else reads (SSH private keys, secrets)
|`770`          |  `rwxrwx---`         | Owner+group: all; others: nothing (shared team directory)
| `000`         | `---------`          | Nobody can do anything


>sudo chmod 770 /opt/webdata

>ls -ld /opt/webdata

>sudo chmod 000 /opt/webdata

>ls -ld /opt/webdata
#### Now even root-owned operations by the wrong user will be blocked

>sudo chmod 777 /opt/webdata

>ls -ld /opt/webdata

⚠️ BLAST RADIUS — chmod 777 

`chmod 777` means everyone on the system — every user, every process, every

compromised account — can read, write, and execute the target.

Engineers reach for 777 when they do not know the correct permission. That is the
wrong instinct. The correct move: identify who actually needs access, set the right
owner/group, then set the minimum permission that makes the thing work.

`chmod 777` on a web server's document root is a direct path to defacement.

`chmod 777` on a config file means any process that runs can overwrite your config.

`The fix for "permission denied" is never 777`. It is diagnosing the correct
owner and permission, then setting exactly that.

### Common patterns every platform engineer knows

|  Scenario                    |   Command                               |   
|------------------------------|-----------------------------------------|
|Make a script executable      |`chmod +x script.sh or chmod 755 script.s`|
|Standard file permissions     |`chmod 644 file.txt`                     |
|Private key file              |`chmod 600 ~/.ssh/id_rsa`                |
|Shared team directory         |`chmod 770 /opt/shared`                  |
|Fix "Permission denied" on a service's data dir|`chown serviceuser:servicegroup /var/data && chmod 750 /var/data`|

### Checkpoint
After this module:

   * Read ls -l output and explain every character
   * Use chmod in both symbolic and octal mode
    * Fix a "Permission denied" error by identifying owner mismatch and correcting it

 * Explain why `chmod 777` is always wrong to a classmate

