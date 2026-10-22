# Phone Backup

**Scope:** how photos actually travel from
a phone to the server — what the app does
automatically, what it refuses to
duplicate, and the platform quirks. This
chapter is documented, not demonstrated:
no phone took part in this guide's build,
and it says so wherever a claim rests on
the project's documentation alone.

## The shape of it

Install the Immich app (Android or iOS),
point it at your server's address, sign in
with the account from chapter 5, and turn
on backup. From then on the app uploads
new photos and videos from the albums you
chose — when you open or resume the app,
and periodically in the background,
subject to each platform's rules below.

Three behaviours decide whether this feels
trustworthy, and all three are deliberate:

- **You choose the albums.** Backup is
  per-album: camera roll in, screenshots
  out, if that's your ruling. On iOS,
  where a photo can live in several
  albums, an album can be *excluded* even
  if another album includes the photo.
- **Copies are deduplicated by content.**
  Before uploading, the app fingerprints
  each file (a checksum of its contents)
  and asks the server, in bulk, "do you
  already have this?" A photo that arrived
  earlier — by web upload, by the
  command-line tool, from another phone —
  is skipped, not copied twice. Moving to
  a new phone therefore doesn't re-upload
  your life's history.
- **Wi-Fi only, unless you say otherwise.**
  The default is to upload on Wi-Fi and
  wait patiently on mobile data. The
  setting to change it is in the app's
  backup settings.

## What lands where

An uploaded photo becomes an asset in
your library: original file into the
upload folder (chapter 2), a database
record, and a queue of jobs behind it —
thumbnails, then the machine-learning
look-overs of chapter 8. Nothing about the
phone's folder structure is imposed on
the server; the timeline orders by the
photo's own date, which the server reads
from the file's metadata.

Optionally, **backup album
synchronisation** mirrors your phone's
albums as albums on the server, one-way:
phone organisation flows up, server edits
don't flow back down. It can be switched
on after the fact and backfilled over
photos already uploaded.

## Deleting, in both directions

The question everyone asks second: if I
delete it *here*, does it vanish *there*?
The safe summary: Immich's backup is not
a two-way sync (chapter 1). Deleting a
photo on the phone leaves the server's
copy alone — that is the point of a
backup. Deleting inside Immich puts the
asset in Immich's trash, from which it is
removed permanently after the retention
period (30 days by default), and that is
a server-side act the phone knows nothing
about. Your phone's own trash and your
server's trash are separate countries;
treat a tidy-up in one as no promise
about the other.

## Platform honesty

- **Android:** the background worker is
  at the mercy of the phone maker's
  battery discipline, and some brands kill
  it aggressively — the project's docs
  point at the "Don't kill my app" guide
  for exempting Immich. You can also tell
  Immich to run its background upload only
  while charging, and to wait a set time
  after a photo is taken before uploading.
- **iOS:** background work requires
  Background App Refresh to be on, and
  Apple — not the app — decides when
  background tasks run. Opening the app
  more often genuinely gets you more
  background runs. Photos that exist only
  in iCloud are pulled down to the phone
  temporarily to be hashed and uploaded,
  which uses real storage and data while
  it happens.

**Check (for your own install):** take a
photo, wait for the app to show it backed
up, then find it in the web timeline at
the photo's date. Then search the server
for the same file uploaded from the web
uploader — the duplicate check should
refuse the second copy. Both behaviours
are the design working, not luck.
