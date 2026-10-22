# Troubleshooting

**Scope:** symptom → likely cause → fix,
in the order a household actually meets
them. The standing first move for
anything on this page: `docker compose
ps` and `docker compose logs` in the
install folder — four boxes, and the logs
say which one is unhappy, in its own
words.

**The web page won't load at all.**
→ Is the stack up? `docker compose ps`
shows fewer than four containers, or the
server restarting in a loop. Read
`docker compose logs immich-server`: the
server refuses to start without its
database and cache, so a database that
failed (disk full, wrong password in
`.env` after an edit) presents as a dead
web page. Fix the named thing, then
`docker compose up -d`.

**Uploads work, but thumbnails, faces, or
search never happen.**
→ The job queues aren't draining — look
at Administration → Job Queues. If counts
only grow, the cache (chapter 2) is the
usual suspect: restart the stack and
watch the queues fall. If *only* the
clever jobs stall while thumbnails flow,
the machine-learning container is the
one: check it is up, and check its logs
for a model download that failed or a
processor it refuses to run on (chapter
8's x86-64-v2 floor).

**"My photos uploaded but search finds
nothing by meaning."**
→ Smart search needs every photo's CLIP
embedding *and* a working ML service at
search time. On a fresh import the
embeddings are simply still in the queue
— this is the normal-hours-to-days state,
not a fault. Search by filename, place,
or date works immediately, which divides
"not yet" from "never."

**An external library scans to zero
files.**
→ Almost always sight, not content. The
container can't see the folder: check
the volume is mounted on the
`immich-server` service, the import path
is the path *as the container sees it*,
and prove it with
`docker exec -it immich_server ls <path>`.
Permissions on the host folder are the
runner-up; the `:ro` mount flag is the
innocent bystander — leave it on.

**A file I deleted from an external
library is still showing.**
→ It shows until the next scan notices;
scans run nightly, or start one from the
library's page. After the scan the asset
sits in trash for the retention period —
that delay is chapter 7's design, working.

**Faces are recognised but two clusters
are obviously the same person.**
→ Merge them from either person's page;
the algorithm's caution is tunable
(Administration → Settings → Machine
Learning → Facial Recognition) but
merging by hand is one click against a
library-wide re-run. The project's own
advice is to nudge these settings, not
crank them — chapter 8 has the reasoning.

**Everything is slow since the big
import.**
→ The database wants fast, local storage;
if `DB_DATA_LOCATION` sits on a spinning
disk or — worse — a network share, every
screen pays for it. Move the database to
an SSD (stop the stack, move the folder,
fix `.env`, start) and let the photos
stay on the big slow disk, where they are
happy. The requirements page is blunt
about this for a reason.

**The machine-learning container exits
immediately, everything else fine.**
→ On an older PC processor, suspect the
x86-64-v2 floor (chapter 3): the ML image
needs it since v3. Your options are the
honest set — run without ML (chapter 8
lists what that costs: people and meaning
search only), point the server at an ML
service on a newer machine, or give the
job to newer hardware.

**A restore produced broken or missing
photos.**
→ The two halves disagreed: the database
was restored from a different moment than
the files (chapter 9's ordering exists
precisely to prevent this). Files present
but unknown can be re-uploaded; database
entries pointing at absent files are the
bad direction — restore the matching
dump, the one that lives beside those
files in `UPLOAD_LOCATION/backups`.

**It worked until I upgraded.**
→ Check the release notes for *your*
jump, not just the latest: breaking
changes are announced per release, and
the jump from an old install can cross
several. This is why chapter 3 pins
versions and chapter 9 keeps dumps: taken
together, "upgrade" becomes "try the new
version, with a way back," which is the
only kind of upgrade a photo library
should ever have.
