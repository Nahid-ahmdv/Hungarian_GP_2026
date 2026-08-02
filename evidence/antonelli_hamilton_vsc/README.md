# Evidence manifest: Antonelli–Hamilton VSC article

The two evidence files shared across the project, documented in
`evidence/README.md`, contain a race-control message log and a
position-observation log for the race session. They support the project in
different ways: the race-control response is used across all three
notebooks, while the position response supports Notebook 1's
position-identity reconstruction. Notebook 3 needed additional evidence:
a finer-grained gap-to-leader series for both drivers across the stop and
VSC sequence, together with direct pit and stint records. This subdirectory
holds that evidence, collected specifically for
`notebooks/03_hungarian_gp_2026_antonelli_hamilton_vsc_article.ipynb`.
It remains separate because these six files are used only by Notebook 3.

**OpenF1 is an unofficial, community-run API.** It is not affiliated with
Formula 1, the FIA, or any team. It is used here only for reproducible,
timestamped context (intervals, pit, and stint records), not treated as an
official source. FIA documents and official Formula 1 reporting take
precedence for classification, penalties, and rules, consistent with the
citation hierarchy in the article notebook itself.

All six files below are preserved verbatim: the exact bytes returned by the
API, with no reformatting, filtering, or recomputation. That was confirmed
with a byte-for-byte `diff` against the original retrieval at copy time.

Session verified: `session_key=11342` matches the session key already
present in the parent `evidence/openf1_hungary_2026_race_control.json` and
`evidence/openf1_hungary_2026_position.json` files. Driver-number mappings
(Antonelli = 12, Hamilton = 44) were independently verified against a fresh
FastF1 session load (`session.results`) before use, not assumed.

---

## File 1: `openf1_intervals_driver_12.json`

- **Endpoint**: `https://api.openf1.org/v1/intervals`
- **Query parameters**: `session_key=11342`, `driver_number=12`, `date>=2026-07-26T14:10:00`, `date<=2026-07-26T14:32:00`
- **Fully resolved request URL**: `https://api.openf1.org/v1/intervals?session_key=11342&driver_number=12&date>=2026-07-26T14:10:00&date<=2026-07-26T14:32:00`
- **Retrieval timestamp (UTC)**: 2026-07-31T09:20:16.179591+00:00
- **HTTP status**: 200
- **Driver number**: 12 (Kimi Antonelli)
- **Session key**: 11342
- **File format**: JSON array of interval-observation records
- **Row count**: 331
- **Byte count**: 46,789
- **SHA-256**: `da5271603d5434449e5a5cf7e696ebc816764d411de1b1c3f737038656daaac5`
- **Fields used by the notebook**: `date`, `driver_number`, `gap_to_leader`
- **How the notebook uses it**: paired against Hamilton's own series
  (File 2) to reconstruct the synchronized-sample ANT-HAM gap across the
  stop and VSC window. The pairing brackets an observed sign change; it
  does not, on its own, prove the white-line mechanism behind it. That
  explanation comes from the cited official Formula 1 and FIA sources, not
  from these interval records.
- **Preserved verbatim**: yes (verified with `diff` at copy time)

## File 2: `openf1_intervals_driver_44.json`

- **Endpoint**: `https://api.openf1.org/v1/intervals`
- **Query parameters**: `session_key=11342`, `driver_number=44`, `date>=2026-07-26T14:10:00`, `date<=2026-07-26T14:32:00`
- **Fully resolved request URL**: `https://api.openf1.org/v1/intervals?session_key=11342&driver_number=44&date>=2026-07-26T14:10:00&date<=2026-07-26T14:32:00`
- **Retrieval timestamp (UTC)**: 2026-07-31T09:20:17.188858+00:00
- **HTTP status**: 200
- **Driver number**: 44 (Lewis Hamilton)
- **Session key**: 11342
- **File format**: JSON array of interval-observation records
- **Row count**: 333
- **Byte count**: 47,220
- **SHA-256**: `b2dc9a0ea01b552af3e787883e1196862103afdea39744c34fb94e4a52780b9a`
- **Fields used by the notebook**: `date`, `driver_number`, `gap_to_leader`
- **How the notebook uses it**: the other half of the same reconstruction
  described under File 1, paired against Antonelli's series. Like File 1,
  it brackets a sign change rather than establishing why the change
  happened.
- **Preserved verbatim**: yes (verified with `diff` at copy time)

## Interval data quality

Both interval files were inspected directly before use. Neither contains a
non-null, non-numeric `gap_to_leader` value.

- Driver 12: 331 raw rows, 3 with a null `gap_to_leader`, 328 usable
  numeric rows.
- Driver 44: 333 raw rows, 3 with a null `gap_to_leader`, 330 usable
  numeric rows.

## File 3: `openf1_pit_driver_12.json`

- **Endpoint**: `https://api.openf1.org/v1/pit`
- **Query parameters**: `session_key=11342`, `driver_number=12`
- **Fully resolved request URL**: `https://api.openf1.org/v1/pit?session_key=11342&driver_number=12`
- **Retrieval timestamp (UTC)**: 2026-07-31T09:20:18.237783+00:00
- **HTTP status**: 200
- **Driver number**: 12 (Kimi Antonelli)
- **Session key**: 11342
- **File format**: JSON array of pit-stop records
- **Row count**: 2 (laps 22 and 53)
- **Byte count**: 361
- **SHA-256**: `76e7953e86ef757939ee3a30cfdcdfa55f76f94355c9838b141be965edef236d`
- **Fields used by the notebook**: `date`, `lap_number`, `lane_duration`,
  `stop_duration`
- **How the notebook uses it**: Antonelli's lap-53 `lane_duration`, the
  total time his car spent in the pit lane, used as context alongside
  Hamilton's equivalent figure rather than as a headline gap measurement.
  `pit_duration` is present in the raw file but superseded by
  `lane_duration` under OpenF1's own current-versus-deprecated field
  distinction. `stop_duration`, the stationary service time within that
  lane duration, is null in this record, so the notebook cannot separate
  stationary time from the rest of the stop, and no pit-lane figure here is
  treated as the amount of time saved under the VSC.
- **Preserved verbatim**: yes (verified with `diff` at copy time)

## File 4: `openf1_pit_driver_44.json`

- **Endpoint**: `https://api.openf1.org/v1/pit`
- **Query parameters**: `session_key=11342`, `driver_number=44`
- **Fully resolved request URL**: `https://api.openf1.org/v1/pit?session_key=11342&driver_number=44`
- **Retrieval timestamp (UTC)**: 2026-07-31T09:20:19.095713+00:00
- **HTTP status**: 200
- **Driver number**: 44 (Lewis Hamilton)
- **Session key**: 11342
- **File format**: JSON array of pit-stop records
- **Row count**: 3 (laps 13, 30, and 56)
- **Byte count**: 541
- **SHA-256**: `a12160cfb2a0be514dc9351ec5ea30ee320d7965a03cb6fb7b145c10afb5e425`
- **Fields used by the notebook**: `date`, `lap_number`, `lane_duration`,
  `stop_duration`
- **How the notebook uses it**: Hamilton's lap-56 stop is the VSC-window
  stop this article is about. Its `lane_duration` is compared directly
  against Antonelli's lap-53 figure from File 3. `stop_duration` is null
  here too, for the same reason as File 3, so the comparison stays at the
  level of total pit-lane time rather than stationary service time, and is
  not read as a measurement of time saved by the VSC.
- **Preserved verbatim**: yes (verified with `diff` at copy time)

## File 5: `openf1_stints_driver_12.json`

- **Endpoint**: `https://api.openf1.org/v1/stints`
- **Query parameters**: `session_key=11342`, `driver_number=12`
- **Fully resolved request URL**: `https://api.openf1.org/v1/stints?session_key=11342&driver_number=12`
- **Retrieval timestamp (UTC)**: 2026-07-31T09:20:19.783628+00:00
- **HTTP status**: 200
- **Driver number**: 12 (Kimi Antonelli)
- **Session key**: 11342
- **File format**: JSON array of stint records
- **Row count**: 3
- **Byte count**: 437
- **SHA-256**: `eec32dc0c8e2ba867d467a85d05acb81232614ce620d723e15fa05ccb0150332`
- **Fields used by the notebook**: `stint_number`, `lap_start`, `lap_end`,
  `compound`, `tyre_age_at_start`
- **How the notebook uses it**: cross-checks Antonelli's compound and
  stint boundaries against the independently loaded FastF1 session.
- **Preserved verbatim**: yes (verified with `diff` at copy time)

## File 6: `openf1_stints_driver_44.json`

- **Endpoint**: `https://api.openf1.org/v1/stints`
- **Query parameters**: `session_key=11342`, `driver_number=44`
- **Fully resolved request URL**: `https://api.openf1.org/v1/stints?session_key=11342&driver_number=44`
- **Retrieval timestamp (UTC)**: 2026-07-31T09:20:20.605409+00:00
- **HTTP status**: 200
- **Driver number**: 44 (Lewis Hamilton)
- **Session key**: 11342
- **File format**: JSON array of stint records
- **Row count**: 4
- **Byte count**: 580
- **SHA-256**: `e3714c877e2a1aebae6bb4f12838588a3ecc21a889e2f6796d47c7bf9aecd2b6`
- **Fields used by the notebook**: `stint_number`, `lap_start`, `lap_end`,
  `compound`, `tyre_age_at_start`
- **How the notebook uses it**: cross-checks Hamilton's compound and stint
  boundaries, including the `tyre_age_at_start=3` value on his final
  stint, against the independently loaded FastF1 session.
- **Preserved verbatim**: yes (verified with `diff` at copy time)

---

## Validation performed before copying into the repository

For every file above, the following were checked against the raw response
before it was copied from the temporary retrieval directory into this
repository:

- HTTP status was 200.
- The response parsed as valid JSON.
- The response was non-empty (row count > 0).
- Every row's `session_key` equals `11342`.
- Every row's `driver_number` equals the requested driver.
- The field set matched the fields documented at
  <https://openf1.org/docs/> for that endpoint (no missing or unexpected
  fields).
- For endpoints containing a `date` field (`intervals` and `pit`), every
  value parsed as a valid ISO-8601 UTC timestamp. The `stints` endpoint does
  not contain a `date` field.
- For the two `intervals` files, no duplicate `date` values were found, and
  every row's `date` fell inside the requested
  `2026-07-26T14:10:00`–`14:32:00` UTC window (0 rows outside the window in
  both files).
- The `pit` and `stints` files contain the expected rows corresponding to
  the stops and stint boundaries independently established from the fresh
  FastF1 session load: Antonelli's lap-53 stop and lap 54–70 HARD stint,
  and Hamilton's lap-56 stop and lap 57–70 SOFT stint.

No response was empty, malformed, or inconsistent; nothing was fabricated,
hand-entered, or copied from Notebook 2 as a substitute.
