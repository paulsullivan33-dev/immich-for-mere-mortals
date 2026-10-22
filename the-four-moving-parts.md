# The Four Moving Parts

**Scope:** the anatomy — the server, the
database, the cache, and the machine-learning
service — what each one holds, what breaks
when each one stops, and how to watch them.

Immich is not one program. It is four,
running side by side, started together by
Docker Compose from the project's official
file. Readers of the Docker Compose guide
in this series already know the shape: one
file, several **services**, each service a
**container** — a program in a sealed box
with its own view of the machine. Here the
four boxes are these:

| Part | Container in the official file | Job |
|---|---|---|
| The server | `immich_server` | The app itself: web page, API, uploads, thumbnails |
| The database | `immich_postgres` | Remembers everything: users, albums, photo metadata |
| The cache | `immich_redis` | Short-term memory: job queues and session odds and ends |
| Machine learning | `immich_machine_learning` | The clever eyes: faces, and search by meaning |

## The server

The server is the part you talk to. It
serves the web interface on port **2283**,
answers the phone apps, receives uploads,
generates thumbnails and preview images,
transcodes video for playback, and hands
out the work it doesn't do itself. When
people say "my Immich is down," this is
usually the box they mean — but not always
the box that's at fault, which is why the
parts have separate names.

It keeps nothing precious of its own. Its
container can be deleted and recreated —
that is literally how upgrades work — and
everything it knows is either in the
database or in the photo files on disk.

## The database

Immich's memory is a **PostgreSQL**
database — the same database family as the
PostgreSQL guide in this series — run in a
special build the project publishes itself
(`ghcr.io/immich-app/postgres`), because it
needs two extensions ordinary PostgreSQL
doesn't ship: one that stores the numeric
fingerprints machine learning produces
(VectorChord, with pgvectors underneath) so
"search by meaning" can be a database
query at all.

Everything except the photo files
themselves lives here: user accounts and
their password hashes, albums and who
they're shared with, every photo's name,
date, camera details, the map position if
it has one, which faces were found, and
the fingerprints for search. **Lose this
database and you still have the photo
files — and have lost the library**: the
albums, the people, the organisation, the
accounts. That sentence is the reason
chapter 9 exists.

## The cache

The third box is **Redis** — or, in the
current official file, its open-source
sibling **Valkey**, which speaks the same
language; the container is still named
`immich_redis` and the server cannot tell
the difference. Its job is short-term:
holding the **job queues** (the to-do
lists of thumbnails to generate, faces to
look for, videos to convert) and passing
small messages between parts of the server.

The cache is the one part that is allowed
to forget. If it stops, uploads stall and
jobs pile up, but nothing precious is
lost; restart it and the server picks its
queues up again. Chapter 12 starts here
more often than anywhere else, because
"Immich is up but nothing is happening" is
usually a queue that isn't draining.

## Machine learning

The fourth box is a separate program
whose only job is looking at pictures. It
runs **models** — pattern-matching files
trained on enormous numbers of images —
that turn a photo into a list of numbers
(a fingerprint called an **embedding**)
such that photos with similar fingerprints
mean similar things. That one trick powers
both headline features: **face detection**
(find the faces, fingerprint them, group
the ones that match into people) and
**smart search** (type "dog on a beach,"
fingerprint the *sentence*, and find the
photos whose fingerprints are nearby).

It is deliberately a separate box because
it is the hungry one. It is a Python
program with its own runtime and model
files (kept in a Docker **volume** named
`model-cache`, so they download once), it
wants far more memory and processor than
the other three, and it can run on a
different machine entirely — chapter 8
covers what works, what waits, and what
simply doesn't happen when it is slow,
absent, or out of memory. The important
honesty, up front: **with the machine-learning
box off, Immich still backs up, browses,
and shares perfectly well.** What you lose
is faces and meaning-search. The photos
were never its job.

## How they talk, and how you watch

On a normal install the four boxes share a
private pretend network that Compose
builds for them, and find each other by
service name — the server asks for the
database at the name `database`, the cache
at `redis`. Only one door faces outward:
the server's port 2283. The database's and
cache's ports stay inside the pretend
network, which is a safety property worth
noticing in chapter 3.

Watching the stack is one command, run in
the folder with the compose file:

```console
$ docker compose ps
```

**Check:** four containers listed, each
`Up`, the server and machine learning and
database marked `(healthy)` once they have
settled — the official file gives them
health checks, little self-tests Compose
reports for you. `docker compose logs
immich-server` shows the server's own
diary; chapter 12 teaches reading it.
