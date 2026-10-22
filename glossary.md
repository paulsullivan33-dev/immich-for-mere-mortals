# Glossary

The terms this guide uses, collected in
one place. Each is explained in the
chapter that leans on it; these are the
short versions.

**Admin** — the first account created on a
new Immich, and the account that manages
the server: creating other users, changing
server-wide settings. Later accounts are
ordinary users unless an admin promotes
them.

**API** — the programming interface the
server offers: a set of web addresses
(`/api/...`) that the web page, the phone
apps, and this guide's lab all talk to.
If the app can do it, the API can do it.

**Asset** — one photo or video in Immich's
records, together with everything the
database knows about it. The file on disk
is the asset's *original*.

**Container** — a program running in a
sealed box with its own view of the
machine, started from an *image*. Immich's
four parts are four containers.

**Docker Compose** — the tool that starts
a set of containers from one text file
(`docker-compose.yml`) describing all of
them. Immich's official install method.

**Embedding** — the list of numbers a
machine-learning model produces from a
photo (or a sentence). Similar pictures
get similar numbers, which is what makes
search by meaning possible.

**External library** — a folder of photos
that already exists on the server's disk —
an old archive, say — which Immich watches
and shows in the timeline *in place*,
without copying the files into its own
storage.

**Face detection / recognition** —
*detection* finds faces in a photo and
marks where they are; *recognition*
fingerprints each face and groups matching
fingerprints into a **person**.

**Job** — one unit of background work:
generate this thumbnail, look for faces in
that photo, convert this video. Jobs wait
in queues held by the cache until a worker
picks them up.

**Machine learning (ML)** — in this guide,
always the fourth container: the program
that runs the models for face detection,
face recognition, and smart search.

**Original** — the photo or video file
exactly as it arrived, kept under the
upload folder. Thumbnails and previews are
derived from it and can be rebuilt;
the original cannot.

**Smart search** — search by meaning
rather than by filename or date: you type
words, the model fingerprints them, and
the database finds photos whose
fingerprints are close.

**Storage template** — the rule that
decides the folder and file names Immich
uses when it files originals in its own
library, for example by year and month.

**Thumbnail** — a small stand-in image
Immich generates from each original so
timelines load quickly. Derived data:
deletable, rebuildable, never the only
copy of anything.

**Upload folder** (`UPLOAD_LOCATION`) —
the place on the server's disk where
originals, thumbnails, and encoded videos
live. Set in the `.env` file at install
time. This is the folder chapter 9 backs
up alongside the database.

**VectorChord** — the PostgreSQL extension
in Immich's own database image that stores
embeddings and answers "which of these is
closest?" quickly. The reason the database
container is a special build.
