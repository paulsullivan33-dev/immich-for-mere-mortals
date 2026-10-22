# Backing Up

**Scope:** the chapter the whole guide
points at — why an Immich backup is *two*
jobs (the database and the originals),
what Immich already does for one of them,
the order the project tells you to take
them in, and what a restore actually
involves.

## The two halves, again

Everything chapter 2 said converges here:

- **The originals** — the photo and video
  files, under `UPLOAD_LOCATION`. Immich
  itself never copies these anywhere for
  safety. Their backup is *your* job,
  with whatever tool you already trust —
  rsync, restic, Borg, a second disk and
  a calendar. (This series' Backups and
  rsync guides are the machinery.)
- **The database** — every account,
  album, face, and fact. Lose it and the
  files survive as an unorganised pile;
  the library — the thing you actually
  built — is gone.

A backup that holds only one half is a
story about a restore that half-worked.
Hold both, from roughly the same moment.

## The half Immich does for you

Immich dumps its own database
automatically: by default **daily at
2:00 AM, keeping the last 14 dumps**, as
compressed SQL files in
`UPLOAD_LOCATION/backups`. The schedule
and retention live in Administration →
Settings → Database Dump Settings, and a
dump can be triggered by hand from
Administration → Job Queues → Create job
→ Create Database Dump.

Read that paragraph again and notice the
address: the dumps land *inside*
`UPLOAD_LOCATION` — the same disk as the
photos. They protect you against a
corrupted database and a bad upgrade,
which is genuinely valuable, and against
nothing else. A dead disk takes the dumps
with the library. **The dumps become a
backup only when your filesystem backup
carries them off the machine** — which
it does automatically if it takes the
whole `UPLOAD_LOCATION` tree, one more
reason to back the tree up whole rather
than cherry-picking.

## The filesystem half

Back up all of `UPLOAD_LOCATION` if you
can. If space forces a choice, the
project's own list of what is *critical*
— the originals, which cannot be rebuilt
— is:

| Folder | What it holds |
|---|---|
| `upload/` (or `library/` with a storage template) | The original photos and videos |
| `profile/` | Users' avatar images |
| `backups/` | The database dumps from the section above |

The rest — `thumbs/`, `encoded-video/` —
is derived data. Skip it and a restore
costs you waiting while the server
regenerates thumbnails and re-converts
videos, which for a large library is
measured in days of processor time, not
minutes. Most people back up everything
and skip the dilemma. And the standing
warning, in the project's own spirit:
**never tidy, rename, or delete files
inside these folders by hand.** Immich
tracks them in its database; a file it
expects and can't find is a broken asset.

## The order, and the quiet method

Database and files copied at different
moments can disagree — the project calls
out both directions of the argument and
picks one:

- **Best:** stop the server while you
  back up (`docker compose stop
  immich-server`). Nothing changes
  mid-copy, so the halves cannot disagree.
- **Otherwise:** **database first, files
  second.** The worst case is then files
  the database doesn't know about —
  recoverable by re-uploading them. The
  other order can leave the database
  describing files your backup doesn't
  have, which is the broken kind of
  surprise.

Since the automatic dumps already happen
on their own schedule, the practical
reading is: let the dumps be, and let
your filesystem job run *after* 2 AM,
taking dumps and originals together.

## Restoring

Restoring is deliberately unglamorous,
and v3 offers it from the web interface:
put the files back under a fresh
`UPLOAD_LOCATION`, start the stack, and
on the welcome screen choose **Restore
from backup** — or, on a running install,
Administration → Maintenance → Restore
database backup. Immich checks the
folders, lists the dumps it found with
their versions, takes a safety backup of
the current database first, restores, and
rolls back by itself if the restore
fails. The command-line route (a
`pg_dump` out, a `psql` back in, against
a database the server has never started
on) is documented by the project for the
automated-recovery crowd.

Two version truths: a dump restores most
happily into the **same Immich version**
that made it, and restoring an older
version's dump into a newer install makes
the restore run the database migrations —
supported, but one more reason chapter 3
pins versions instead of floating.

**Check (the only one that counts):** a
backup you have never restored is a
hope, not a backup. Once, on a spare
machine or a second folder, restore your
dumps and files and sign in. Count the
photos. Look at an album. That afternoon
is the difference between *having* a
photo service and *hoping* you have one.
