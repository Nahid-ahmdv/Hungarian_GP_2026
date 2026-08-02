# Shared OpenF1 evidence manifest

This directory holds two OpenF1 API responses kept as shared supporting
evidence for the project's notebooks: a race-control message log and a
position-observation log. Both were retrieved for the 2026 Hungarian Grand
Prix race session, which OpenF1 identifies as `session_key=11342`; the
notebooks load the corresponding race independently through FastF1. Each
file is a raw, unmodified copy of what the API returned. The notebooks cite
or read these files but never rewrite them, and the SHA-256 checksums below
identify the exact bytes used as evidence. No causal or counterfactual
conclusion is drawn from this manifest itself; it exists to make the
underlying evidence traceable, not to interpret it.

OpenF1 is an unofficial, community-run API. It is not affiliated with
Formula 1, the FIA, or any team. Official FIA documents, FIA regulations,
and official Formula 1 reporting take precedence over anything in these
files for rules, penalties, classification, and the official description
of what happened on track.

## File 1: `openf1_hungary_2026_race_control.json`

- **OpenF1 endpoint**: `https://api.openf1.org/v1/race_control`
- **Exact query parameters**: `session_key=11342`
- **Retrieval timestamp (UTC)**: 2026-07-28T15:57:51.666339+00:00
- **File format**: JSON, an array of race-control message records
- **SHA-256 checksum**: `3c49153ee1e66ecc19fc7fe8bf50554e7e5733a14f39735e5b63fdf2d5e62cff`
- **How it is used**: this file records the Virtual Safety Car deployment
  message (`2026-07-26T14:22:55Z`, lap 56, `VSC DEPLOYED`) and the ending
  message (`2026-07-26T14:24:13Z`, lap 57, `VSC ENDING`). All three
  notebooks rely on these timestamps for VSC timing checks. Notebook 1
  cites this file directly when cross-checking its own FastF1-derived VSC
  window (Module E and the Stage 2 verdict table); Notebooks 2 and 3 open
  and read it in code, comparing it against the equivalent timestamps
  FastF1's own race-control table already carries, to confirm the two
  sources agree before either notebook relies on them.

## File 2: `openf1_hungary_2026_position.json`

- **OpenF1 endpoint**: `https://api.openf1.org/v1/position`
- **Exact query parameters**: `session_key=11342`
- **Retrieval timestamp (UTC)**: 2026-07-28T15:57:59.878617+00:00
- **File format**: JSON, an array of position-observation records
- **SHA-256 checksum**: `dbccb527abfa476444bc3b1b979d48290e338fc55ff11e5c103b2b3a1f10692d`
- **How it is used**: this file supports Notebook 1's reconstruction of who
  actually held second place around Norris's VSC-window pit stop, car 44
  (Hamilton) recorded at position 2 at `2026-07-26T14:22:30.439Z`, and car
  3 (Verstappen) recorded at position 2 at `2026-07-26T14:23:50.308Z`. That
  identity check matters because a before-and-after comparison against an
  unlabeled "P2" would otherwise silently compare two different drivers.
  Notebook 1 cites this file in Module E and in its Stage 5 shortlist entry
  for Candidate C3.

## Preservation and provenance

Both files are kept here as verbatim copies of their original OpenF1
responses, with no reformatting, filtering, or recomputation applied. The
checksums above identify precisely which bytes are being described; if
either file is ever refreshed or re-retrieved, a changed checksum is the
signal that the notebooks referencing it need to be re-checked against the
new content.

## Not included

The official FIA Final Race Classification, Document 62, is referenced by
the notebooks but is intentionally not duplicated in this directory. It is
linked directly to the official FIA source:
<https://www.fia.com/system/files/decision-document/2026_hungarian_grand_prix_-_final_race_classification.pdf>
