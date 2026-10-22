# Writing Criteria

Every guide in this repo is written to the same
standard. If you're adding or editing a guide —
human or AI — hold it to these.

1. **Plain words, real ideas.** Simple language,
   never dumbed-down concepts. Explain jargon on
   first use; collect terms in the glossary.
2. **No hype.** No marketing adjectives, no
   "revolutionary," no promises about what
   something will do for the reader.
3. **Honest about limits.** Say what the thing
   *can't* do. State uncertainty instead of
   smoothing it over. Validation notes include
   what remains unverified.
4. **No invented numbers.** Benchmarks, defaults,
   and specs are either measured or qualified with
   "depends on your setup." If it wasn't tested,
   say so.
5. **Checkable.** Commands are meant to be run,
   with "Check:" blocks showing what success looks
   like. Sources are linked, not asserted.
6. **Respect the reader's setup.** Call out
   Windows/Mac/Linux differences. Don't assume a
   GPU, a budget, or prior experience.
7. **Safety isn't skipped.** Real risks get
   stated plainly, not buried.
8. **Scoped.** Each guide says what it covers and
   points elsewhere for the rest. No guide
   pretends to be the whole subject.
9. **Structure over prose.** Tables, before/after
   examples, symptom → fix. Scannable beats essay.

## Verification notes for this guide

Built and verified 2026-10-22 against
**Immich v3.3.5** (GitHub release published
2026-10-21, the current release on build
day).

**Primary sources read, not remembered:**
the release's official `docker-compose.yml`
and `example.env` (reproduced in
`examples/`); the published OpenAPI spec
from the v3.3.5 tag (193 paths — every
endpoint the chapters name was checked
against it); and the project's own
documentation for requirements, backup and
restore, mobile backup, libraries, facial
recognition, and smart search.

**Ran for real** (transcripts in
`immich-lab/transcripts/`): the four
published images — server and
machine-learning v3.3.5, the official
file's VectorChord PostgreSQL 14 image,
and Valkey 9 — were fetched from ghcr.io
and their amd64 manifests verified by
digest (server `sha256:1ab81816…`, ML
`sha256:1270449e…`). The build machine's
sandbox refuses the `setns` call every
container runtime needs, so Docker could
not start them; the images' filesystems
were run directly instead, with their own
entrypoints and environment, sharing the
host network — the deviation chapter 3
declares. PostgreSQL initialised and
accepted connections, Valkey answered, the
ML service answered its ping, and the
server ran its whole migration chain
without an error, then:

- t00 — `/server/ping` → `{"res":"pong"}`;
  `/server/version` → 3.3.5.
- t01 — admin sign-up created the invented
  admin (`isAdmin: true`).
- t02 — login returned a token (redacted
  in the transcript).
- t03 — specimen photo upload →
  `status: "created"` and a server-given
  asset id.
- t04 — the asset read back with its
  checksum, dimensions job done
  (`resized: true`).
- t05 — server statistics: 1 photo,
  21,316 bytes, attributed to the admin.
- t06 — the stored file found under
  `upload/`; its SHA-1 matches the
  specimen sent, exactly.
- t07 — the same photo uploaded again →
  `status: "duplicate"`, same id, no
  second copy.
- t08 — an external library created over
  a specimen folder, scan accepted
  (HTTP 204), library statistics 1 photo;
  server statistics then showed 2 photos
  while stored bytes stayed 21,316 — the
  library's files were indexed in place,
  not copied, as chapter 6 describes.
- t09 — ML service ping → `pong`.

**Attempted, and failed honestly:** a
`refresh-faces` job re-queued after the
session was accepted (HTTP 204) and the ML
service twice tried to download its
`buffalo_l` model; the download did not
produce a usable model in this sandbox and
the job failed. So facial recognition and
smart search are described from the
project's documentation and from the
request the server was observed sending —
no inference result is claimed anywhere in
this guide. Thumbnails and metadata
extraction did run (see t04).

**Documented only, and labelled as such
in the chapters:** phone backup (no phone
on the bench — chapter 5 follows the
project's manual and the spec); the web
interface (the lab drove the API, which
the chapters state); backup restoration
(chapter 8 teaches the project's
documented procedure and the database/
files distinction the lab's own folders
demonstrate; no restore was rehearsed);
hardware-accelerated transcoding and ML.

**Fixtures:** the admin (Marta Ellison,
marta@example.com) and both specimen
photos are invented and were generated on
the build machine. Passwords and tokens
are redacted in every transcript.
