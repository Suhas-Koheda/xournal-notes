# Xournal++ notebooks

Handwritten notes synced between the laptop and the phone through this
repository.

## How the sync works

```
laptop  ~/Documents/XournalNotes   <-- git -->  GitHub  <-- git -->  phone
                                                                     /sdcard/Documents/XournalNotes
```

* **Laptop**: `notes-open` pulls, launches Xournal++, and pushes when you close
  it. `notes-sync` does the same by hand. `notes-sync status` shows what is
  where.
* **Phone**: Xournal++ Mobile in *Local storage* mode points at
  `/sdcard/Documents/XournalNotes`. Termux runs `~/bin/notes-sync.sh` from
  cron (via `crond`) every 15 minutes, which pulls and pushes with real git.
  No laptop or USB connection is involved.

## PDF copies

Every sync on the laptop exports a PDF for each notebook that changed, into
`pdf/` next to the notebook. Xournal++ is not available on Android, so the
phone uploads `.xopp` only and the laptop fills in the PDFs.

## Conflict policy

A `.xopp` file is zipped/gzipped XML, so git cannot merge it. Neither side ever
tries:

* on a conflict the **remote version becomes the notebook**
* the other version is kept next to it as
  `Name (conflict <machine> <timestamp>).xopp`

Open the conflict copy, copy over anything missing, then delete it. Nothing is
ever thrown away automatically.

## Housekeeping

`*.xopp~`, `*.autosave.xopp` and `*.xopp.bak` are Xournal++ working files and
are ignored by git. Only real notebooks are committed.

## Laptop setup on a new machine

```sh
git clone <this repo> ~/Documents/XournalNotes
cd ~/Documents/XournalNotes
git config merge.notes-xopp.driver "$HOME/bin/notes-merge %O %A %B %L %P"
git config pull.rebase false
```

`notes-merge` is the driver that turns a conflict into two files instead of
corrupting one. Keep it in `~/bin`.

## Phone setup (already done)

```sh
pkg install git cronie termux-services
crontab -l            # */15 * * * * $HOME/bin/notes-sync.sh
sv-enable crond
termux-wake-lock
```

The cron daemon starts with the Termux session. After a reboot, open Termux
once (or install Termux:Boot) so the schedule resumes.
