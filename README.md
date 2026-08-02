# 2026 Hungarian Grand Prix Story Analysis

This repository records a research process built around the 2026 Hungarian
Grand Prix, looking for a story strong enough to support a full article.
That process happened in stages: a broad audit of the race came first, then
a separate follow-up on candidates that turned up after the audit had
already closed, and only after that a focused article notebook built around
one of those candidates.

The chronology is preserved honestly rather than smoothed over after the
fact. The first notebook searched broadly and did not find a standalone
story. The second notebook tested three further candidates on their own
terms and, while each held up as a smaller finding, none of them was
promoted to a standalone article either. The third notebook takes one of
those candidates and develops it into the article this project set out to
produce.

## The three notebooks

1. [`notebooks/01_hungarian_gp_2026_story_discovery.ipynb`](notebooks/01_hungarian_gp_2026_story_discovery.ipynb)
   is the original story-discovery audit. It works through FastF1 lap,
   stint, sector, position, track-status, and race-control data, checks
   data quality, sets explicit lap-eligibility rules, and screens a wide
   range of candidate stories before validating and closing them one by
   one. Its final verdict is `NO MAIN-ARTICLE-STRENGTH STORY IDENTIFIED`.
   Several observations held up along the way (a penalty-driven
   classification change, Verstappen's extended final stint, a repeatable
   Leclerc-Hamilton sector pattern), but none combined enough originality,
   evidentiary strength, and race consequence for a standalone piece.

2. [`notebooks/02_hungarian_gp_2026_external_candidate_followup.ipynb`](notebooks/02_hungarian_gp_2026_external_candidate_followup.ipynb)
   is a separate, later notebook. After Notebook 1 closed, three further
   candidates surfaced through search that had not been part of the
   original discovery pipeline. Rather than fold them back into Notebook
   1's methodology after the fact, this notebook screens them on their own,
   against the same promotion criteria. Its final dispositions: the
   McLaren second-stop inversion (E1) and the Antonelli-Hamilton VSC
   sequence (E2) both close as `SECONDARY / SHORT-FORM`; the Verstappen
   traffic-position finding (E3) closes as `CONTEXTUAL FINDING ONLY`. None
   of the three reached `STANDALONE ARTICLE` in this notebook either.

3. [`notebooks/03_hungarian_gp_2026_antonelli_hamilton_vsc_article.ipynb`](notebooks/03_hungarian_gp_2026_antonelli_hamilton_vsc_article.ipynb)
   develops candidate E2 into the article this project produced, titled
   "How Hungary's VSC Compressed Antonelli's Tyre Gamble." It adds evidence
   neither of the first two notebooks used: OpenF1 interval, pit, and stint
   records collected specifically for the article, for both drivers. With
   that evidence, it reconstructs the ANT-HAM lap-boundary sequence lap by
   lap, brackets the observed sign change in a synchronized-sample
   reconstruction of the same gap, and keeps pit-lane duration separate
   from whatever race-time benefit the VSC produced. Where the pit-lap
   numbering could be read two ways, the convention this notebook follows
   is stated up front rather than left implicit.

The mechanism behind the lap-58 position change, a control-line review
rather than an overtake, was already documented by Formula 1 and the FIA
before this notebook was written. Notebook 3 does not claim to have
discovered it. What it adds is a bounded, reproducible quantitative
reconstruction of the sequence around that change, built from timing data
rather than from officially published prose, along with an explicit
separation between what the timing data show and what the official record
explains.

## Evidence organization

Two evidence manifests sit alongside the notebooks.
[`evidence/README.md`](evidence/README.md) documents the two shared OpenF1
files. The race-control response is used across all three notebooks for VSC
timing checks, while the position response supports Notebook 1's
position-identity reconstruction.
[`evidence/antonelli_hamilton_vsc/README.md`](evidence/antonelli_hamilton_vsc/README.md)
documents six additional OpenF1 files collected specifically for Notebook 3:
interval, pit, and stint records for both drivers.

OpenF1 is an unofficial, community-run API. It is not affiliated with
Formula 1, the FIA, or any team, and every notebook treats it that way: as
reproducible, timestamped context rather than an authoritative source.
Where OpenF1 and an official account might seem to overlap, FIA documents,
FIA regulations, and official Formula 1 reporting take precedence for
classification, penalties, rules, and any explanation of what a
control-line or race-control decision actually meant.

## Repository structure

```
.
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   ├── 01_hungarian_gp_2026_story_discovery.ipynb
│   ├── 02_hungarian_gp_2026_external_candidate_followup.ipynb
│   └── 03_hungarian_gp_2026_antonelli_hamilton_vsc_article.ipynb
└── evidence/
    ├── README.md
    ├── openf1_hungary_2026_race_control.json
    ├── openf1_hungary_2026_position.json
    └── antonelli_hamilton_vsc/
        ├── README.md
        ├── openf1_intervals_driver_12.json
        ├── openf1_intervals_driver_44.json
        ├── openf1_pit_driver_12.json
        ├── openf1_pit_driver_44.json
        ├── openf1_stints_driver_12.json
        └── openf1_stints_driver_44.json
```

## Environment and execution

Each notebook was developed and tested on Python 3.11, specifically
3.11.15. Dependencies are pinned in
[`requirements.txt`](requirements.txt):

```bash
python3.11 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

To make that environment available to Jupyter under a predictable kernel
name, register it once:

```bash
python -m ipykernel install --user \
  --name hungarian-gp-2026 \
  --display-name "Hungarian GP 2026"
```

All three notebooks use FastF1's local cache to avoid re-downloading timing
data on every run. The cache location resolves in order: the
`FASTF1_CACHE` environment variable if it is set, otherwise
`$XDG_CACHE_HOME/fastf1/Hungarian_GP_2026`, and finally
`~/.cache/fastf1/Hungarian_GP_2026` if neither is set. To use a specific
location:

```bash
export FASTF1_CACHE=/path/to/your/preferred/cache
```

Each notebook is independently executable and should be run top to bottom
in a clean kernel; later cells depend on objects created earlier in the
same notebook, not on any other notebook. To run one interactively, launch
Jupyter, select the `Hungarian GP 2026` kernel, and open the notebook from
the UI. To execute one cleanly from the command line instead, for example:

```bash
jupyter nbconvert \
  --to notebook \
  --execute \
  --inplace \
  --ExecutePreprocessor.kernel_name=hungarian-gp-2026 \
  --ExecutePreprocessor.timeout=1800 \
  notebooks/03_hungarian_gp_2026_antonelli_hamilton_vsc_article.ipynb
```

This runtime override selects which installed kernel actually executes the
notebook; it does not alter the notebooks' own stored kernel metadata,
which stays the generic `Python 3` shown in each file.

The notebooks are included with their executed outputs already in place,
so reading them does not require running anything first. Reproducing those
outputs does still require the dependencies listed above and access to
FastF1's underlying data source; on a first, uncached run this can take
noticeably longer than a later run against a warm cache.

## Interpretation boundaries

Across all three notebooks, three kinds of statements are kept apart on
purpose: values read directly from FastF1 or the preserved OpenF1 evidence,
values calculated locally within a notebook (paired deltas, sensitivity
checks, synchronized-sample reconstructions, and similar diagnostics), and
analytical interpretation of what those values might mean. A local
calculation is not an official Formula 1 or FIA value on its own, and an
interpretation is not presented as settled fact beyond what the cited
evidence actually supports.
