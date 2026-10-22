# Limits

**Scope:** the honest edges — what Immich
doesn't do, what it does slowly, and the
assumptions baked into it that will matter
exactly when one of them stops being true
for you.

Every guide in this series has this
chapter in some form. Immich's limits are
not defects so much as the visible seams
of its design: a household photo service,
built by a community, that runs on one
machine in your house.

**It is a backup target, not a backup.**
Immich receives and keeps photos, and it
makes database dumps (chapter 9), but the
safety of the whole pile — originals and
database alike, on a second disk or off
the premises — is arranged by you, with
other tools. An Immich server with no
backup behind it is one disk failure
away from being a story.

**One machine, one point of failure.**
The four parts expect to live together.
There is no built-in way to spread them
across a cluster, and if the server is
off, the library is off — phones queue
their photos and wait. For a household
this is normally fine; it is worth knowing
it is the design, not a missing setting.

**The clever features are the slow
features.** On processor alone, face
detection, recognition, and smart-search
embedding proceed at whatever pace your
CPU allows, and a lifetime's import is
measured in days of background work, not
an evening (chapter 8). The service is
usable throughout — the timeline fills
immediately — but "why can't I search for
'dog' yet" has an honest answer, and it
is *the queue*.

**It changes underneath you.** Immich
releases frequently and occasionally
rearranges its own furniture: settings
move, features graduate from experimental,
a major version (like v3, this guide's)
adjusts requirements. Pinning a version
and reading release notes before upgrading
(chapters 3 and 9) is not paranoia; it is
the project's own expectation of its
operators. The database backup *process
itself* changed in v2.5.0 — restores from
older dumps follow older instructions —
which is the fact to remember, not the
numbers.

**Faces are grouped, not known.** The
people feature clusters similar faces; it
does not identify anyone, it will split
one person into two clusters and merge
strangers' children, and every name in it
is one a user typed. It also means your
server holds biometric-shaped data —
embeddings of your family's faces — which
is a reason this lives on your disk and
not a stranger's, and a reason to think
before sharing an admin account.

**Deleting means deleting eventually.**
Trash is a waiting room (30 days by
default), thumbnails and encoded videos
of a deleted asset go with it on the
server's schedule, and an external
library's file deleted on disk follows
its own path through the same trash
(chapter 7). "I deleted it" and "it is
gone from every disk" are separated by
design, on purpose, in your favour —
until the day you wanted it gone *now*.

**Not an editor, not a DAM.** Beyond
rotation and the basics, Immich does not
edit photos, and it is not a professional
digital-asset manager: no colour-managed
proofing, no tethered shooting, no
licensing metadata workflows. Photographers
with those needs keep their existing
tools and may still use Immich as the
family-facing window — the neighbours
chapter names the usual partners.

**Your disk, your ceiling.** A photo
library grows in one direction. Immich
adds its tenth-to-a-fifth overhead of
thumbnails and converted video, the
database runs to gigabytes at household
scale, and the machine-learning models
want their share. None of this is hidden
— `UPLOAD_LOCATION` is a folder you can
watch — but the plan for "the disk is
full in three years" is yours to make
while it is still cheap to make.
