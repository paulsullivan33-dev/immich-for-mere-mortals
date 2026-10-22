# External Libraries

**Scope:** the other way photos get in —
pointing Immich at folders that already
exist on the server's disk, so an old
archive joins the timeline *in place*,
without being copied into Immich's own
storage.

## Why this exists

Most households sitting on a photo
problem don't have a phone problem; they
have a *folder* problem: twenty years of
pictures in a directory tree on some disk,
from cameras, scanners, and older phones.
Uploading all of that through a browser
would duplicate every file into Immich's
upload folder. An **external library**
instead says: these files, in this folder,
are the originals — show them alongside
everything else.

Once scanned, an external photo behaves
like any other asset: it appears in the
timeline, on the map if it carries a
position, in albums, in search. The file
itself never moves.

## Setting one up

Three pieces, in order:

1. **Give the server sight of the folder.**
   A container sees only what Compose
   mounts into it, so the folder gets a
   volume line on the `immich-server`
   service, alongside the upload folder:

   ```yaml
   volumes:
     - ${UPLOAD_LOCATION}:/data
     - /srv/photos-archive:/mnt/archive:ro
   ```

   The `:ro` — read-only — is the part to
   copy carefully: with it, Immich can
   look but cannot delete, and nothing in
   the web interface can damage the
   archive. Leave it off and you have
   given the software write access to your
   only copies; the project's own example
   marks exactly that line with a warning.

2. **Create the library** (an admin task:
   Administration → External Libraries).
   Choose which *one* user owns it — a
   library belongs to a single user, and
   that choice can't be changed later.
   Add one or more **import paths**, using
   the paths as the *container* sees them
   (`/mnt/archive`, not the host path you
   mounted from), optionally with
   **exclusion patterns** — globs like
   `**/Raw/**` to skip a folder of camera
   raw files, or `**/*.{arw,dng}` to skip
   the formats themselves.

3. **Scan.** The scan walks the import
   paths and creates an asset for every
   photo and video it finds. A scan also
   runs automatically once a day (the
   interval is adjustable), and filesystem
   *watching* — noticing new files the
   moment they land — exists but is marked
   experimental by the project itself,
   and generally can't work over network
   drives.

**Check:** from inside the container, the
folder must be listable before any scan
will find it —
`docker exec -it immich_server ls /mnt/archive`
shows your files, or chapter 12 wants a
word. Nearly every external-library
failure is a mount or a permission, and
this one command divides the two.

## The honest edges

External libraries come with fine print
that matters, all of it from the
project's own documentation:

- **The files are the library.** Delete
  or move a file on disk and the next
  scan notices: a deletion sends the
  asset to Immich's trash (restoring the
  file restores the asset); a move makes
  the photo a *new* asset at its new
  path.
- **Immich-side notes live only in
  Immich.** Add an external photo to an
  album or write its description, and
  that knowledge sits in the database,
  not in the file. Because a moved file
  returns as a new asset, its album
  memberships and descriptions do *not*
  follow it. Curate the archive's
  organisation in the folders themselves,
  and treat Immich's layer as a bonus.
- **Nothing is copied, so nothing is
  backed up by Immich.** The archive's
  safety is still your rsync, your
  restic, your second disk — Immich adds
  a window onto the folder, not a copy of
  it. Chapter 9's backup jobs cover
  Immich's own folders; the archive needs
  its own line in the same plan.
- **Other users see it only by sharing.**
  Ownership is one user; housemates get
  the pictures through shared albums, the
  same as uploads.

## When to use which door

Uploads (chapters 5 and 6) are for
*arriving* photos: phones, new cameras,
anything whose home will be Immich's own
storage. External libraries are for
*resident* photos: an archive with an
existing home and an existing backup
story, which you want searchable and
browsable without doubling its footprint.
Plenty of real installs run both — the
old tree mounted read-only, the phones
uploading into fresh storage — and the
timeline cheerfully mixes the two.
