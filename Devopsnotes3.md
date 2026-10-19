# Module 1.3 — Editors


The problem this solves
Production servers have no GUI. If you need to edit a config file on a remote machine, you edit it in the terminal. This module gives you two tools: nano (Week 1 workhorse) and enough vim to survive (not get stuck).
nano — the beginner-safe editor
nano shows its controls at the bottom of the screen. You do not have to memorise commands.

## Open or create a file
>nano myfile.txt

Inside nano:

    Type normally to edit
    Ctrl+O — save (WriteOut); press Enter to confirm filename
    Ctrl+X — exit (asks to save if unsaved changes)
    Ctrl+W — search (Where Is)
    Ctrl+K — cut a line
    Ctrl+U — paste (uncut)
    Arrow keys — move around

Use nano for all your Week 1 editing. It does not have a learning curve.

## vim — the survival kit
vim is the most common editor you will find on servers. You will encounter it even if you never choose it — some tools drop you into vim automatically (git commit, crontab, visudo). You need to know how to get out.

## The two modes that matter
vim starts in Normal mode. In Normal mode, keys are commands, not text.

To start typing text:
  Press i       → enters Insert mode

  [type your changes]

To stop typing and go back to Normal mode:
  Press Esc

To save and quit (from Normal mode):
  Type :wq      → write (save) and quit
  Press Enter

To quit without saving (from Normal mode):
  Type :q!      → force quit, discard changes
  Press Enter

To save without quitting:
  Type :w
  Press Enter

That is your Week 1 vim survival kit. You do not need more than this yet.
## Practical: edit the hostname

### Check current hostname
> hostname 
 > cat /etc/hostname 

### Edit it (use nano in Week 1)
>sudo nano /etc/hostname
### Change the value, Ctrl+O to save, Ctrl+X to exit

## Apply the change without rebooting
>sudo hostname newname
>hostname

### What gets skipped and why

    vim motions, macros, splits, registers — powerful, but not Week 1 scope
    emacs — not in scope for this bootcamp; name it so students know it exists
    IDE-style editors (VS Code remote, JetBrains Gateway) — valid for later work,

but Week 1 establishes the terminal-only skill first
Full vim fluency is a legitimate career investment. The resource pointer for post-bootcamp: vimtutor (built into most Linux systems — type vimtutor in the terminal and work through it at your own pace).
### Checkpoint
### After this module:

    Edit a file with nano without getting stuck
    Escape vim if you land in it accidentally
    Know the difference between :wq and :q!

### Further Reading

    OpenVim — Interactive Tutorial — OpenVim (core)
    MIT Missing Semester: Editors — MIT (further)
    Vim Cheat Sheet — vim.rtorr.com (further)

