# What Immich Is

**Scope:** the mental model — a photo and
video backup service you run yourself, with
the shape of Google Photos and the files on
your own disks — plus what it is not, and
the lab this guide's claims rest on.

## The one-paragraph version

**Immich** is open-source software that
backs up the photos and videos on your
phone to a computer in your own house, and
then gives you a web page and a phone app
that look and feel a great deal like Google
Photos. You browse a timeline, search,
build albums, share with family. The
difference is where the pictures live and
who holds the keys: the originals sit in a
folder on your disk, the whole thing runs
on hardware you own, and no account with
any company is involved. It began, by its
author's own account, because he wanted
that shape of service without handing a
lifetime of family photos to an advertising
company — and it is now a large, actively
developed project with many contributors.

That is the whole idea. The rest of this
guide is the fine print: the four programs
that make it run, what the clever features
actually cost in computing power, and the
part most people get wrong — that keeping
the photos safe is *two* jobs, not one.

## The shape of the thing

Three pieces of the experience are worth
naming before any machinery:

- **Backup is the point.** Immich's first
  job is getting the only copy of a photo
  *off* your phone and onto a second
  machine. Everything else — timelines,
  faces, search — is decoration on top of
  that one act of copying.
- **The originals stay ordinary files.**
  An uploaded photo lands in a folder
  tree on the server's disk as a normal
  file, readable by any program. Immich
  also builds thumbnails and keeps a
  database of everything it knows, but it
  does not lock your pictures inside a
  private format. If Immich vanished
  tomorrow, the photos would still be
  sitting there as files.
- **Each person has their own account.**
  A household runs one Immich; each family
  member signs in as themselves, sees
  their own library, and can share albums
  with the others. The first account
  created is the **admin** — the person
  who manages the server. Chapter 5 makes
  one.

## What it is not

Four confusions worth clearing away early:

- It is **not a sync tool**. Sync (like
  the Syncthing guide in this series)
  keeps folders identical everywhere,
  including deletions. Immich is a backup
  and browsing service: the phone sends,
  the server keeps. Deleting on the phone
  does not silently delete the server's
  copy — chapter 6 explains the exact
  behaviour, because it matters.
- It is **not a photo editor**. It crops
  and rotates in the viewer and that is
  roughly where editing ends. Its job is
  keeping and finding, not fixing.
- It is **not free of machinery**. Google
  hides its servers; Immich shows you
  yours. There are four moving parts
  (chapter 2), and when one stops, the
  service stops. This guide treats that
  as a fair trade, stated plainly.
- It is **not finished software in the
  old sense**. Immich releases often and
  still changes its own furniture between
  versions. Pinning the version you run,
  and reading the release notes before an
  upgrade, are standing habits — chapter 3
  builds both in from the start.

## Versions and the lab

This guide was built against **Immich
v3.3.5**, the current release on the day
of the build, from the project's own
published pieces: the official
`docker-compose.yml` from that release
and the four container images it names,
fetched from the project's registry and
checked against their published digests.
On top of it the lab made one invented
admin account, uploaded invented photos
generated on the build machine (no
person's real pictures were involved
anywhere), and exercised the project's
published **API** — the same interface
the web page and phone app talk to — to
check what the server claimed against
what it did.

One sandbox quirk, stated once: the build
machine's kernel forbids the namespace
join every container runtime needs, so
Docker could not start the stack there.
The lab ran the four images' filesystems
directly instead — same binaries, same
entrypoints, same folders the compose
file names, sharing the host's network.
Chapter 3 tells the story in full. On any
normal machine, Docker Compose runs the
file as published and none of this
applies. `criteria.md` keeps the full
ledger of what was run and what was only
read.

Check: you should now be able to say what
Immich copies, where the copies live, and
why "the app works" and "the photos are
safe" are two different statements. If that
is clear, chapter 2 opens the box.
