# Installing with Docker Compose

**Scope:** the official install, start to
first sign-in screen — what you need, the
two files you download, the handful of
lines you edit, and the command that
starts all four parts at once.

## What you need

Immich's published requirements, stated
plainly: a 64-bit Linux machine (the
project is blunt that Docker on other
systems gives a poor experience), **6 GB
of memory minimum, 8 recommended**, **2
processor cores minimum, 4 recommended**,
and Docker with the Compose plugin — the
machinery from this series' Docker and
Docker Compose guides. Storage wants a
Unix-style filesystem (EXT4, ZFS, and
friends), and the database in particular
must live on a local disk, never a network
share. Expect the library to grow by
roughly a tenth to a fifth beyond the
photos themselves, for thumbnails and
converted videos.

One hardware footnote that bites older
machines: since v3, the machine-learning
container on ordinary (amd64) processors
needs the **x86-64-v2** instruction set —
nearly everything since about 2012 has
it, but a genuinely old spare-parts box
may not, and chapter 8 is where that
shows up.

## The two files

Make a folder to hold the install, and
download the two files the project
publishes with every release:

```console
$ mkdir immich-app && cd immich-app
$ wget -O docker-compose.yml https://github.com/immich-app/immich/releases/latest/download/docker-compose.yml
$ wget -O .env https://github.com/immich-app/immich/releases/latest/download/example.env
```

That is the whole download. The compose
file describes the four services from
chapter 2 with their images and plumbing;
the `.env` file holds the settings you are
expected to choose. (`curl -o` in place of
`wget -O` does the same job; Windows and
Mac readers get both files from the same
release page in a browser.)

## The lines you edit

Open `.env`. Four settings matter on day
one; the file itself marks the line below
which nothing needs changing:

| Setting | What it decides |
|---|---|
| `UPLOAD_LOCATION` | Where the photos live — the big folder. Put it on your largest, safest disk. |
| `DB_DATA_LOCATION` | Where the database files live. Local SSD, never a network share. |
| `DB_PASSWORD` | The database's own password. Change it from the example — letters and digits only, per the file's own warning. |
| `IMMICH_VERSION` | Which release to run. The example pins the current major line; pinning an exact version (this guide's lab used `v3.3.5`) means upgrades happen when *you* say so. |

Two decisions in that table deserve a
slow moment. `UPLOAD_LOCATION` is the
folder chapter 9 backs up and the folder
you will feed with a decade of photos;
moving it later is possible but tedious,
so point it at the disk you actually mean.
And the password: it is a real credential
guarding the database, not a formality.
Generate one, keep it in your password
manager, and never paste it into a chat
or a screenshot.

## Starting it

```console
$ docker compose up -d
```

Compose downloads the four images (the
machine-learning one is the large one;
the first start is a coffee, not a
sprint), creates the pretend network,
and starts everything in dependency
order — cache and database first, server
after them. Watch it settle:

```console
$ docker compose ps
$ docker compose logs -f immich-server
```

The server's log ends, on a good start,
by announcing it is listening and that
the web interface is ready. Then open
`http://<your-server>:2283` in a browser —
the first screen of chapter 5, offering
to create the admin account.

**Check:** `docker compose ps` shows four
containers up, and the browser shows an
Immich welcome screen rather than an
error. If the page refuses to load, the
server log is the first place to look —
chapter 12 walks the common failures.

## The lab's declared deviation

This guide's lab could not use Docker at
all, and the reason belongs on the record:
the build machine runs in a sandbox whose
kernel refuses the namespace-joining call
(`setns`) that Docker — and every other
container runtime tried — needs before a
container's first process can start. The
images themselves arrived without trouble
and were checked against their published
digests, so the lab ran them the plain
way: each image's filesystem mounted with
its own `/proc` and `/dev`, the same data
folders the official file names bound
into the same places, each service started
with its image's own entrypoint and the
`.env` values above — with the addresses
pointed at `127.0.0.1`, since all four
shared the host's network — in the
official order of birth. What answered
this guide's API calls is the project's
own v3.3.5 code, byte for byte; only the
box around it was missing. On any normal
machine, follow the file exactly as
published and skip this section; the lab's
transcripts in `immich-lab/` and the
ledger in `criteria.md` show precisely
what ran.

## Leaving it running, and stopping it

`restart: always` is in the official file
for every service, so the stack comes
back by itself after a reboot or a crash.
`docker compose down` stops and removes
the containers — your photos and database
stay on disk, in the two folders from
`.env`, untouched. That pair of facts is
the whole lifecycle: the containers are
disposable, the folders are the install.
