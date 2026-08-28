---
name: conservative-docs-copyedit
description: |
  Run a conservative, objectively-wrong-only copy-edit pass over the docs/ tree
  of this repo (Canonical Sphinx starter pack). Use when asked to review docs for
  wording, grammar, typos, or "improper wording" — including noun/verb confusions
  like setup vs set up — without introducing style churn. Covers the allowed and
  forbidden change categories, the reStructuredText mechanics that break the
  build, the baseline-diff verification recipe, and the parallel review contract.
---

# Conservative docs copy-edit

The docs in `docs/` have a few recurring error patterns (a/an before acronyms, space before colon or comma,
number+noun compounds left unhyphenated). The recurring *risk* is an over-eager
reviewer rewriting perfectly acceptable prose. This skill exists to keep the diff
defensible: every changed line must be wrong, not merely improvable.

## Rule of engagement

One test for every edit: **could a reviewer say "that was objectively
incorrect"?** There are exactly two grounds:

1. It is grammatically, semantically, or mechanically wrong.
2. It violates an established convention in the documentation.

The convention's *initial choice* can be subjective; after the repo owner,
product vendor, or an established local pattern has made that choice, deviation
is an objective defect. For example, choosing `DisplayPort` over `DP` is a
style decision; leaving `DP port` after `DisplayPort` is established is
incorrect. Do not invoke "style" to preserve a known inconsistency.

If the only defence is "this reads better", do not change it — record it under
`SKIPPED` instead. If the text looks factually or technically wrong rather than
grammatically wrong, never edit it; record it under `SUSPECT` and report it to
the user.

## Consistency doctrine

Before normalising a variant, establish the pattern rather than inventing one.
Use this evidence hierarchy, in order:

1. **Explicit repo-owner ruling in this skill or the current request.**
2. **Official product/vendor spelling** for a product or trademark.
3. **An established local pattern** — the same document or closely related
   sibling documents use one form consistently for the same meaning.
4. **An established repo-wide documentation pattern.**
5. **An existing project check** (Vale, spelling, or Sphinx) that requires a
   form and does not conflict with higher-priority evidence.

Same spelling does not imply the same meaning. Do not count identifiers, URLs,
commands, package names, filenames, generated output, or official product
names as prose precedent. A `#` comment in a code block is prose. If evidence
is mixed or no pattern is established, leave the text alone and report the
candidate under `SKIPPED`; do not create a convention by copy-edit.

## Allowed categories

Numbered so change logs can cite them.

1. **Article agreement (pronunciation-based)** — `an USB` -> `a USB`,
   `an USB-C` -> `a USB-C`, `a NFS mount` -> `an NFS mount`, `a LTS` -> `an LTS`,
   `a SD card` -> `an SD card`. Note `a UEFI` but `an EFI`, `an SBAT`.
2. **Noun vs verb compounds** — `to setup X` -> `to set up X`; `the set up` ->
   `the setup`. Same for log in/login, back up/backup, boot up/bootup.
3. **Subject-verb agreement** — `Jetson Orin Nano don't have` -> `doesn't have`.
4. **Missing required article** — `Ubuntu image has been tested` -> `The Ubuntu
   image has been tested`; `on display` -> `on the display`; `pressing Escape or
   F11 key` -> `pressing the Escape or F11 key`; `using NVIDIA package
   repository` -> `using the NVIDIA package repository`.
5. **Wrong comparative/conjunction** — `for the same reasons than above` -> `as
   above`; `prefer a real disk than a stick` -> `over a stick`.
6. **Wrongly pluralised mass nouns** — `firmwares` -> `firmware`,
   `informations` -> `information`. Also the reverse: fixed-plural idioms, e.g.
   `for development purpose` -> `for development purposes`.
7. **Number+noun compound modifiers** — `2 pin jumper cap` -> `2-pin jumper
   cap`, `12 pins Button Header` -> `12-pin Button Header`. Only when the
   number+noun modifies a following noun.
8. **Space before punctuation** — `enter the following command :` -> `command:`,
   `Thermal zones : Some` -> `Thermal zones: Some`,
   `https://launchpad.net/ubuntu , create` -> `ubuntu, create`.
9. **Typos, doubled words, doubled spaces mid-sentence** — `by  running` ->
   `by running`.
10. **Ungrammatical word order** — `You can connect to your developer kit a
    USB keyboard and a monitor using a DisplayPort cable` -> `You can connect a
    USB keyboard and a monitor to your developer kit using a DisplayPort
    cable`.
11. **Indisputably wrong preposition/pronoun** — `insert it on the kit` -> `into
    the kit`; `instructions of NVIDIA JetPack` -> `instructions for`;
    `generate the signature lists and save it` -> `save them`;
    `brings anything necessary` -> `brings everything necessary`;
    `allow to check` -> `allow you to check`.
12. **Distributive plural in a test-setup sentence** — when one accessory is
    described against several devices, both nouns are plural, and the sentence
    leads with what the commands ran *on*, not with the accessory. Ruled
    example: `tested on an IMX219 camera module connected to a Nano and NX
    developer kit` -> `tested on Orin Nano and NX developer kits with
    IMX219 camera modules connected`. Two defects in one: singular `kit` for
    two kits (each has its own camera), and the setup described from the wrong
    end. Flag other instances under `SUSPECT` rather than rewriting them —
    this category was ruled on case by case, and restructuring a sentence is
    otherwise forbidden under the Style rule below.
13. **Established-convention violation** — normalise a prose variant when the
    [consistency doctrine](#consistency-doctrine) establishes the required form.
    This includes spelling, capitalisation, terminology, and approved
    abbreviation expansion. Record the evidence in the change log, for example:
    `Display-Port` -> `DisplayPort` — VESA spelling, custom wordlist entry, and
    existing release-note usage. Never use raw frequency as proof; first
    exclude identifiers and cases where the terms mean different things.

## Forbidden — leave alone even if you would write it differently

- **Typography.** Curly vs straight apostrophes and quotes (`’` `“` `”`),
  ellipsis characters, em/en dashes. This tree uses curly quotes in prose; a
  copy-edit pass must not flip them, and this is the mistake automated agents
  make most often. Grep the finished diff for it.
- **Terminology normalisation** is required when the [consistency
  doctrine](#consistency-doctrine) establishes a form. The house-style rulings
  in [Branding](#branding) and [Terminology](#terminology) are binding. Generic
  prose uses lowercase `developer kit`; named products use title case, for
  example `Jetson AGX Thor Developer Kit`.
- **Unruled variant spellings.** Do not switch `customisation` or other UK
  spellings to US English, or vice versa, merely because you prefer one. If a
  documented convention establishes one form for the same prose meaning,
  category 13 applies instead.
- **Style.** Rewording for tone/concision, sentence restructuring beyond
  category 10, Oxford commas, heading capitalisation, list-item trailing
  periods, comma splices, `in order to`, line rewrapping. Docs use one long line
  per paragraph — keep it. This does not protect a deviation from an established
  convention.
- **Everything non-prose.** `.. code-block::` bodies and literal blocks,
  command output, file paths, ``inline literals``, URLs, RST link *targets*,
  `.. _anchor:` labels, `:ref:` targets, substitution names, toctree entries,
  directive/option names. Also `docs/.custom_wordlist.txt` and
  `docs/.sphinx/.wordlist.txt`. `#` comments in code blocks are prose; apply
  established prose conventions to them. Never alter an identifier to make it
  look consistent with prose.
- **Whitespace-only issues** such as trailing whitespace (README.md:34).
- Adding or removing sentences, notes, or admonitions.

## Branding

Vendor brand names are normalised to the vendor's own spelling, everywhere in
prose. This is house style, not taste — do not leave variants mixed, and do not
revert a normalisation:

- **NVIDIA** — never `Nvidia`, `NVidia`, `nVidia`.
- **JetPack** — never `Jetpack`.
- **DisplayPort** — never `Display-Port`, `Display Port`, or the abbreviation
  `DP`. (VESA spelling; also the `docs/.custom_wordlist.txt` entry and the form
  used in both release notes.) Spell it out even where the abbreviation would be
  shorter: `DP` invites redundant expansions like `a DP port` = "a DisplayPort
  port". Name the thing the monitor actually has — ruled example: `A monitor
  with a DP port` -> `A monitor with a DisplayPort input`. Do not "fix" this
  back to `DisplayPort port`.

Scope, in order of precedence:

1. **Prose, headings, and link/reference text** — always normalise. `Nvidia` ->
   `NVIDIA` is length-preserving, so heading underlines never need resizing.
2. **Code blocks, commands, package names, URLs, image filenames, and inline
   literals** — never touch. `nvidia-smi`, `nvidia-l4t-cuda`,
   `linux-nvidia-tegra-jetson`, `repo.download.nvidia.com`,
   `nvcr.io/nvidia/l4t-jetpack:r36.3.0`, `nvidia-smi.png`, `--runtime nvidia`
   are all correct as written; the lowercase spelling is part of the identifier.
3. **`#` comments inside code blocks** — the exception to exception 2: a comment
   is prose, so `# Install gstreamer plugins and nvidia codecs` ->
   `NVIDIA codecs`. `echo` strings that are program output stay as authored.
4. **RST link definition names** (`.. _flashing Jetpack:`) — normalise these
   too. Reference names are matched case-insensitively, so the target and its
   reference text can be updated independently without breaking the link, but
   update both so they read identically. The `--fail-on-warning` build is the
   proof: an unresolved reference fails it.

Audit with a case-sensitive search for the wrong forms; it must come back empty
(`rc=1`). The `--exclude-dir` flags matter: `docs/.sphinx/.wordlist.txt` is a
vendored dictionary that legitimately contains `Nvidia`, and `docs/_build` holds
generated HTML — without them this command reports false positives forever.

```bash
grep -rnP --include=*.rst --include=*.md --include=*.txt \
  --exclude-dir=.sphinx --exclude-dir=_build \
  'Nvidia|NVidia|nVidia|Jetpack|jetPack|Display-Port|Display Port|\bDP\b' \
  docs README.md
```

To find brand words needing a judgement call (prose vs identifier), search for
the standalone word: `(?i)(?<![\w./-])nvidia(?![\w./-])`.

## Terminology

House style, ruled on by the repo owner. Same identifier and file-name carve-out as
[Branding](#branding): never rewrite a command, package, URL, anchor label,
filename, or ``inline literal``; `#` comments in code blocks *are* prose and
do get fixed.

- **GRUB** — always `GRUB`, never `Grub` or `grub`, including where it names the
  binary shim validates (`secure-boot.rst`: "used to validate GRUB and the
  kernel"). It is an acronym: GRand Unified Bootloader.
- **Prerequisites** — no hyphen. `Pre-requisites` -> `Prerequisites`; the `"""`
  underline shrinks by one character. The singular `Prerequisite` heading is
  fine when a single step follows. **Watch for a label collision:**
  `autosectionlabel` is enabled with document prefixes, so renaming a heading
  into a title that already exists in the same file fails the build with
  `WARNING: duplicate label classic/installation-jammy:prerequisites`.
  `installation-jammy.rst` already had a `Prerequisites` (camera) section, so
  the GStreamer one became `Prerequisites for GStreamer`, matching the file's
  existing `Prerequisites for VPI`; `installation-noble.rst` was renamed the
  same way for symmetry. Before renaming any heading, grep the file for the
  target title and grep the tree for `:ref:` uses of the old label.
- **developer kit** — use lowercase `developer kit` / `developer kits` in
  generic prose, headings, and comments. Use title case only for an explicit
  named product, for example `Jetson AGX Thor Developer Kit` or `Jetson Orin
  Nano Developer Kit`. `Orin NX Developer Kit` is not a product name; bare
  model shorthand in prose, such as "on an Orin NX developer kit" or "on the
  AGX developer kit", takes lowercase. `Developer kit` (capital D, lowercase
  k) is always wrong. Keep `devkit` only inside identifiers and the strings
  that contain them: `jetson-agx-orin-devkit`, `jetson-orin-nano-devkit`,
  `jetson-agx-thor-devkit`, the `.. _devkit-recovery-mode:` anchor, and
  `docs.nvidia.com/jetson/agx-thor-devkit/...` URLs. The heading expansion from
  `devkit` to `developer kit` changes its underline from 33 -> 40.

- **preinstalled** — prose uses `preinstalled`, never `pre-installed`: `flash.rst`
  uses “preinstalled NVMe disk” and “preinstalled image”. Never alter
  `preinstalled` or `pre-installed` in identifiers or filenames.

Audit; must come back empty (`rc=1`). The lookarounds skip identifiers and
filenames, so `jetson-orin-nano-devkit`, `grub-install` and `grub.cfg` do not
match:

```bash
grep -rnP --include=*.rst --include=*.md \
  --exclude-dir=.sphinx --exclude-dir=_build \
  '(?<![\w-])devkits?\b(?!-)|(?<![\w-])development kits?\b|(?<![\w-])Developer kits?\b|\bGrub\b|(?<![\w./-])grub(?![\w./-])|\bPre-requisites\b|(?<![\w./-])pre-installed(?![\w./-])' \
  docs README.md
```

`-P` is required, not `-E`. GNU grep's ERE engine has no lookbehind: with `-E`
this pattern degrades to `grep: warning: ? at start of expression` and exits
`1`, i.e. it reports a clean tree no matter what is in the files. A false pass
is worse than no check. Both audit commands here were verified against a probe
file containing every wrong form plus identifier lookalikes.

The regex can catch only the mixed `Developer kit` form. Whether a
correctly-cased occurrence is title case or lowercase depends on whether it is
a formal product name, which no regex can determine; that call stays with the
reviewer.

## reStructuredText mechanics

- Changing heading text **requires** resizing its underline (and overline, if
  present) to the new length, or `--fail-on-warning` fails the build.
  `Setup secure boot` -> `Set up secure boot` needed `=================` ->
  `==================`; `installation.rst` needed both rules 43 -> 44.
- Image `:alt:` text is prose and is in scope.
- Markdown link labels in README.md are reference-style: changing
  `[Set-up page]` requires changing its definition at the bottom identically.
- Prefer wording already present in the docs so the Vale spell check stays green.

## Verification (mandatory, and cheap)

Capture a baseline from clean `main` **before** editing, because `make vale`
already fails on `main` with 9 pre-existing `Use 'Launchpad' instead of
'launchpad'` errors. Without the baseline you cannot tell your damage from theirs.

```bash
git worktree add /tmp/docs-base main
ln -sfn "$PWD/docs/.sphinx/venv" /tmp/docs-base/docs/.sphinx/venv   # skip a venv rebuild
cd /tmp/docs-base/docs && make spelling && make woke && make vale > /tmp/vale_base.txt
```

After editing, in the real tree:

```bash
cd docs
make spelling                                                        # expect 0 errors
make woke                                                            # expect 0 errors
make html SPHINXOPTS="-c . -d .sphinx/.doctrees -j auto --fail-on-warning"
make vale > /tmp/vale_new.txt; diff /tmp/vale_base.txt /tmp/vale_new.txt
```

`make vale` must differ from the baseline only in **column numbers** (removing a
space before a comma shifts them). Any new message is yours. Finish with
`git worktree remove /tmp/docs-base --force`.

Then audit the diff at character granularity — `--stat` and `-U1` hide quote
flips:

```bash
git diff --word-diff=plain --word-diff-regex='.' -U0
git diff --word-diff=plain --word-diff-regex='.' -U0 | grep -cE "\[-’-\]|\[-“-\]|\[-”-\]"   # must print 0
```

## Parallelising the pass

The tree is ~1,900 lines over ~21 files, so slice by file group and fan out —
`docs/how-to/flash.rst` alone, the rest of `how-to/`, `classic/installation-*`,
all `release-note*`, then README + index/reuse files. Give every agent the
allowed/forbidden lists verbatim, tell it to skip all builds and linters (the
parent runs the suite once), and require this output shape:

```
path:line | category N | "before" -> "after"
EVIDENCE: <required only for category 13; ruling, official spelling, or local pattern>
SKIPPED:  <thing left alone> — <why>
SUSPECT:  <possible factual/technical error, untouched>
```

Category 13 without evidence is not an accepted edit. “More common” is not
evidence until the agent shows that the competing uses are the same prose
meaning rather than identifiers, quoted output, or proper names.

Zero changes is a valid result for a file; say so explicitly.

**Always review what comes back.** Observed failures from a real run: agents
silently flipped `’` to `'` on lines they were otherwise editing, and a category
10 reorder produced a fresh misplaced modifier (`...a monitor using a
DisplayPort cable to your developer kit`). Re-read every reordered sentence.

## Known SUSPECT items already reported (do not re-litigate as wording)

- `docs/how-to/secure-boot.rst` code block: `mv ~/Downloads/{a.der, b.der}` —
  bash brace expansion breaks on the spaces after the commas.
- `docs/classic/release-note-noble-thor-ga.rst`: bug ID 2122571 is attached to
  two different known issues.
- `docs/core/index.rst`: body is just `Release Note.`, reads like scaffold.
