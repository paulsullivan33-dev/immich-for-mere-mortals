# First Admin and Your Library

**Scope:** the first five minutes and the
first photos — creating the admin account,
understanding what lands where when a
photo arrives, and the everyday furniture
of albums, sharing, and trash.

## The first account is the admin

Open the server's address
(`http://<your-server>:2283`) on a fresh
install and Immich offers exactly one
move: **create the admin account** — a
name, an email address, and a password.
Whoever does this first owns the server:
the **admin** can create other accounts,
change server-wide settings, and manage
libraries. On a home network this is
low-drama; on anything reachable from the
internet, do it the minute the stack
starts, because until someone does, the
offer is open to whoever visits first.

This is worth pausing on because it is
the whole security model in miniature:
Immich has no default password to forget
to change — it has no accounts at all
until you make the first one yours.

Users after the first are created by the
admin (Administration → User Management),
each with their own email and password,
each with a private library of their own.
There is no self-registration unless an
admin turns it on.

## Your first upload

Sign in and the empty timeline invites an
upload. Drag photos onto the web page (or
use the upload button), and watch what
"uploaded" actually sets in motion:

1. The original file is stored under
   `UPLOAD_LOCATION/upload/<your user id>/`
   — an ordinary file, byte for byte what
   you sent. (Installations that switch on
   the **storage template** file originals
   under `library/` instead, in folders
   shaped by a rule like year/month; the
   default is off, and chapter 9 explains
   why the address matters.)
2. A **database record** — the asset — is
   created holding everything the server
   knows: filename, dates, camera and lens
   details and position read from the
   photo's own metadata, and a checksum
   that later proves this exact file is
   already known (chapter 6's dedup).
3. **Jobs queue up** behind it: preview
   and thumbnail generation first, video
   transcoding if it was a video, then the
   machine-learning errands of chapter 8.
   Administration → Job Queues shows the
   queues and their counts — the server's
   to-do list, in the open.

The same door exists without a browser:
the project publishes a command-line tool
(`immich`, via npm) that signs in with an
API key and uploads whole folders — the
usual way a lifetime's archive first
arrives, and the thing this guide's lab
exercised through the API directly.

**Check:** after uploading, look in
`UPLOAD_LOCATION/upload/` — the file is
there, and `sha1sum` (or its equivalent)
matches the original you sent. The web
page's claim and the disk's claim agree;
that agreement is the product.

## The everyday furniture

- **Timeline** is the home view: every
  asset, newest first, grouped by date,
  scrubbed by the bar at the right edge.
- **Albums** are named piles you make by
  hand (or the phone creates by mirroring,
  chapter 6). An album can be **shared**
  with other users on your server, and —
  if you choose — by a public link, which
  deserves a slow decision on any server
  the internet can reach: the link *is*
  the permission.
- **Explore** gathers the machine's
  opinions: people (chapter 8), places
  from photo positions, and other ways of
  slicing the same library.
- **Trash** holds deletions for 30 days
  by default before they become
  permanent; emptying it by hand is
  available and irreversible, in that
  order.
- **Favorites and archive** are the two
  soft states: a heart for the keepers,
  and an archive that hides an asset from
  the timeline without deleting it — for
  the screenshots and receipts of life.

## What the lab did here

The build's lab drove this chapter
through the API the apps themselves use:
its requests and the server's answers —
the sign-up, the sign-in, an uploaded
specimen photo reappearing in the asset
list with its server-given identity — are
kept in `immich-lab/transcripts/`, and
`criteria.md` records exactly which steps
ran. The specimen photos were generated
on the build machine; no real person's
pictures were anywhere near this guide.
