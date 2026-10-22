# Immich for Mere Mortals

Somewhere in your house there is a phone
holding the only copy of a thousand
photographs. **Immich** is the open-source
answer to that quiet emergency: a photo
and video backup service you run yourself,
with the shape of Google Photos — timeline,
albums, faces, search by meaning — and the
files on your own disks, under your own
keys. This guide explains how it works,
what its four moving parts each do, how
to install it the official way, and the
part that decides whether any of it
matters on the bad day: the two backups
that are not the same job.

The lab behind this guide ran the real
thing: **Immich v3.3.5**, the project's
current release on build day. Into it went
one invented admin account and a handful
of specimen photos generated on the build
machine — no real person's pictures were
used anywhere — and the server was
exercised through the same **API** its web
page and phone apps talk to: first
sign-up, first upload, the asset
reappearing with its server-given
identity, a folder of existing photos
attached as an external library. What the
lab could not do, it did not fake: there
was no phone on the bench, so phone backup
is documented from the project's own
manual and labelled as such, and the
machine-learning chapters distinguish
carefully between what the build machine
ran and what it merely read. One sandbox
quirk shaped the whole run — the build
machine's kernel forbids the call every
container runtime needs, so the four
services ran from their images' own
filesystems, started directly and sharing
the host's network, instead of inside
Docker containers — declared in full in
chapter 3 and in `criteria.md`, which
keeps the ledger of what was run and what
was only read.

## What this guide covers

- [**What Immich is**](what-immich-is.md) —
  the mental model: backup first, originals
  as ordinary files, one account per
  person; what it is not (a sync tool, an
  editor, a finished antique)
- [**The four moving parts**](the-four-moving-parts.md) —
  the server, the database, the cache, and
  the machine-learning service: what each
  holds, and what breaks when each stops
- [**Installing with Docker Compose**](installing-with-compose.md) —
  the official two files, the four lines
  you edit, starting the stack, and the
  lab's declared deviation, told in full
- [**First admin and your library**](first-admin-and-library.md) —
  the first account is the admin, what an
  upload sets in motion, albums, sharing,
  trash, and the checksum that proves the
  file on disk is the file you sent
- [**Phone backup**](phone-backup.md) —
  album choice, deduplication by content,
  Wi-Fi patience, and the Android and iOS
  fine print (documented — no phone was
  harmed, or used)
- [**External libraries**](external-libraries.md) —
  pointing Immich at the folder you
  already have: mounts, the read-only
  rule, scans, exclusion patterns, and
  the metadata that doesn't travel
- [**Faces and search**](faces-and-search.md) —
  embeddings, the one trick behind people
  and meaning-search; the costs, and what
  still works with the clever box off
- [**Backing up**](backing-up.md) — the
  database *and* the originals: the dumps
  Immich makes for you, the folders that
  are critical, the order to take them
  in, and the restore drill that turns
  hope into a backup
- [**Recipes**](recipes.md) — the standing
  install, the decade import, two phones
  one server, ML on a budget, and the
  Sunday backup
- [**The neighbours**](the-neighbours.md) —
  Google Photos, iCloud, PhotoPrism,
  Nextcloud, Ente, the NAS boxes: what
  each one trades
- [**Limits**](limits.md) — one machine,
  one point of failure; the slow clever
  features; a project that changes
  underneath you, stated plainly
- [**Troubleshooting**](troubleshooting.md) —
  the dead web page, the stuck queue, the
  library that scans to zero, the upgrade
  that bit
- [**Glossary**](glossary.md) — the terms,
  collected in one place

## What it doesn't cover

Running Immich for anyone but a household
— multi-server deployments, SSO and
corporate identity, Kubernetes charts —
and exposing any server to the internet
safely, which is a subject of its own
(reverse proxies and TLS get a warning,
not a tutorial). Photo editing, DAM
workflows for working photographers, and
migrating *out of* a commercial cloud are
each real topics this guide only touches:
the neighbours chapter names the tools,
and your old service's export button is
the start of that story, not this one.

## How to use this guide

Read chapter 1 for the model and chapter
2 before installing anything — knowing
which box is which turns later confusion
into a named problem. Chapter 3 gets a
server running; chapter 5 with a browser
open is the natural next sitting. Readers
with an existing photo archive should not
skip chapter 7: the external-library door
may be the whole reason you came. And
chapter 9 is not the optional ending —
schedule its Sunday recipe before your
first bulk import, while the library is
still small enough to rehearse a restore
in minutes. The lab's transcripts live in
`immich-lab/transcripts/`; its notes are
`examples/lab-notes.txt`.
