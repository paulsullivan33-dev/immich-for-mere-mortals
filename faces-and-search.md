# Faces and Search

**Scope:** the machine-learning chapter —
what the fourth container actually does
when it "looks at" a photo, the two
features that buys you (people, and search
by meaning), what they cost, and what
still works when the whole apparatus is
switched off.

## One trick, two features

Chapter 2 introduced the **embedding**:
a model reads a photo and answers with a
list of numbers, arranged so that photos
which *mean* similar things get similar
lists. Everything in this chapter is that
trick, applied three ways:

- **Face detection.** A model scans the
  photo's preview image and answers a
  simpler question first: *are there faces
  here, and where?* It returns boxes —
  coordinates and a confidence score for
  each face-shaped region.
- **Face recognition.** Each boxed face is
  cropped and passed to a second model,
  which embeds it. Faces whose embeddings
  land close together are grouped by a
  clustering algorithm (a relative of
  **DBSCAN**, in the project's own
  description) into a **person**. By
  default a person needs at least three
  mutually similar faces before the group
  is shown to you — a lone face in a crowd
  stays an outlier rather than becoming a
  one-photo stranger in your People list.
- **Smart search.** A third model (a
  **CLIP** model, which reads images *and*
  text into the same number-space) embeds
  every photo. Type "birthday cake" into
  search and your sentence is embedded
  too; the database returns the photos
  whose numbers are nearest your words.
  No one tagged anything. That is the
  whole magic, and it is genuinely as
  strange as it sounds the first time it
  works.

The embeddings live in the database —
that is what the special PostgreSQL build
and its VectorChord extension are *for* —
while the models live in the ML container,
which downloads them on first use into
its `model-cache` volume. Expect a new
install's first face and search jobs to
wait on those downloads.

## What it feels like, honestly

Faces appear under **Explore → People**:
unnamed clusters at first, each a face
thumbnail over a pile of photos. Naming a
person is your job — Immich groups, it
doesn't know your family. From a person's
page you can merge two clusters the
algorithm kept apart, hide a person, set
a birth date (and then be shown their age
in each photo), or pick which photo is
their portrait. Smart search is just the
search box, doing double duty: it matches
names and places literally, and meanings
approximately.

Quality, stated plainly: the grouping is
very good and not perfect. Twins, babies
growing into teenagers, and a decade of
the same face ageing will all produce
some wrong piles and split piles; the
merge tools exist because the project
knows this. Recognition also runs a
**nightly job** that reconsiders faces it
couldn't place, so people sometimes
improve overnight as more photos arrive.

## The costs

- **Processor.** On a machine without a
  graphics card, every photo's detection,
  recognition, and search embedding is
  CPU work. A big first import queues
  thousands of these jobs; the server
  works through them at its own pace and
  the features simply arrive late. This
  is normal, not broken — chapter 12's
  first lesson.
- **Memory and the model files.** The ML
  container is the stack's heavyweight.
  The project's own guidance is that a
  4 GB machine should run with machine
  learning *disabled* (in Administration
  → Settings → Machine Learning), and the
  models it downloads are real disk
  weight in the cache volume.
- **A newer-processor floor.** Since v3
  the ML image for ordinary PCs needs the
  x86-64-v2 instruction set (chapter 3);
  on an old enough processor the ML
  container exits and the rest of the
  stack carries on without it.

## With the lights off

Nothing in chapters 5–7 depends on any of
this. With machine learning disabled or
the container stopped: uploads, timeline,
albums, sharing, external libraries,
metadata and map (positions come from the
photo files themselves) all work exactly
as before. You lose People, meaning
search, and nothing else — and the jobs
simply aren't queued, so a small machine
stops paying for features it can't
afford. Plenty of careful people run
exactly that way, which is why this guide
treats ML as the fourth part, not the
first.

This guide's lab notes record precisely
how far the build machine got with each
of the three jobs — `criteria.md` has the
ledger, as always, and nothing in this
chapter claims a result the lab didn't
see.
