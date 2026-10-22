# Recipes

**Scope:** five whole jobs, end to end.
Each assembles only things the earlier
chapters have already explained; where a
step is documented rather than something
this guide's lab ran, the recipe says so
in its own text.

## 1. The standing install

Goal: a server that survives reboots and
upgrades without drama. Chapter 3's
install, with the three habits added:
pin `IMMICH_VERSION` to the exact release
you tested; put `UPLOAD_LOCATION` on the
big disk and `DB_DATA_LOCATION` on the
fast one; and write the folder's address
on the household's "where things live"
list, because the person who needs it in
three years is you, mid-crisis. Verify
monthly with chapter 9's check, not with
hope.

## 2. The decade import

Goal: a folder tree of old photos —
scans, camera dumps, a dead laptop's
Pictures folder — into Immich, once,
completely.

1. Decide the door (chapter 7's fork):
   the tree keeps its own home and backup
   story → external library, mounted
   `:ro`. The tree is a staging pile →
   upload it and let Immich own the
   originals.
2. For the upload route, use the CLI
   tool rather than a browser: sign in,
   mint an API key in Account Settings,
   and point `immich upload` at the tree.
   Deduplication (chapter 6) makes a
   second run safe — it skips what
   already arrived, so an interrupted
   import is resumed by simply running it
   again.
3. Expect the queue era: thumbnails in
   hours, faces and search embeddings
   over following days on CPU. Judge the
   import by the *file count on disk*
   matching the tree, not by when search
   gets clever.

## 3. Two phones, one server

Goal: a couple's phones backing up to one
household Immich. One admin account (the
household sysadmin, chapter 5), one
ordinary account per person — libraries
are private by default — then a shared
album for the pictures both want in
common, fed by hand or by the phone
apps' album mirroring (chapter 6). The
decision to make consciously, once: does
the household want one *joint* account
instead? It's simpler and it deletes the
privacy boundary between the two
libraries; most households, asked
directly, pick the boundary.

## 4. The ML on a budget

Goal: faces and search on a small machine
without the timeouts. Three honest
levers, in order: **patience** — let the
queues run overnight for the first
fortnight; **deferral** — in Job Queues,
pause the machine-learning jobs during
the bulk of a big import and resume them
when the thumbnails are done, so the
responsive work isn't queued behind the
slow work; **exile** — the ML container
is the one part that may live elsewhere:
run it on the household's newest machine
and point the server at it
(`IMMICH_MACHINE_LEARNING_URL`), leaving
the old server to do everything it's
good at. What doesn't work is wishing: a
4 GB machine should simply switch ML off
(chapter 8) and skip to being an
excellent plain backup.

## 5. The Sunday backup

Goal: chapter 9 as a habit rather than a
chapter. A weekly job on the server — a
cron entry or a systemd timer, in this
series' own idiom — that, in order:
leaves Immich's own 2 AM database dump to
happen; then copies `UPLOAD_LOCATION` and
`DB_DATA_LOCATION`'s latest dump to the
second disk (and, monthly, to the copy
that lives off the premises). Quarterly,
the restore drill onto a spare folder and
a count of the photos. The recipe's real
content is the calendar entry: backups
fail socially before they fail
technically, and an unscheduled drill is
an undone drill.
