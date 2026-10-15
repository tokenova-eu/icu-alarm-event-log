# Data card: MIMIC-III reference set

A reference set for trying the tools in this repository without access to hospital
data. It is derived only from open-access PhysioNet databases. It contains no data from
the partner hospitals of the project.

## Summary

| | |
| :--- | :--- |
| ICU stays | 31 records from the MIMIC-III Waveform Database Matched Subset |
| Resolution | 1 sample per second (monitor numerics) |
| Length | 1,738,729 seconds in total (about 483 hours); 10.4 to 23.7 hours per stay |
| Signals | mean arterial pressure, systolic arterial pressure, SpO2, heart rate |
| Context | drug administrations and charted alarm limits for one stay; intervals beyond the default alarm limits of a common monitor model for all stays |
| Licence | Open Data Commons Open Database License (ODbL) 1.0, following the source data |

## Sources

| Source | Used for | Licence |
| :--- | :--- | :--- |
| MIMIC-III Waveform Database Matched Subset v1.0, <https://physionet.org/content/mimic3wdb-matched/1.0/> | monitor numerics at 1 Hz | ODbL 1.0 |
| MIMIC-III Clinical Database Demo v1.4, <https://physionet.org/content/mimiciii-demo/1.4/> | drug administrations (INPUTEVENTS_MV) and charted alarm limits (CHARTEVENTS) for the one stay present in both databases | ODbL 1.0 |
| Philips IntelliVue MX400-800 instructions for use, Default Settings Appendix, adult column | default alarm limits used to derive `mimic3_philips_default_limits.csv` | thresholds cited as facts; no text or figures reproduced |

Please cite the source databases when you use this set:

- Moody B, Moody G, Villarroel M, Clifford GD, Silva I. MIMIC-III Waveform Database
  Matched Subset (version 1.0). PhysioNet (2020). <https://doi.org/10.13026/c2294b>
- Johnson AEW, Pollard TJ, Mark RG. MIMIC-III Clinical Database Demo (version 1.4).
  PhysioNet (2019). <https://doi.org/10.13026/C2HM2Q>
- Goldberger A, et al. PhysioBank, PhysioToolkit, and PhysioNet: Components of a new
  research resource for complex physiologic signals. Circulation 101(23):e215-e220 (2000).

## Files

| File | Rows | Content |
| :--- | ---: | :--- |
| `data/mimic3wdb_matched_1hz.csv.gz` | 1,738,729 | `record, offset_s, map_mmhg, spo2_pct, heart_rate_bpm` |
| `data/mimic3wdb_matched_1hz_sys.csv.gz` | 1,738,729 | the same rows plus `sys_mmhg` |
| `data/manifest.csv` | 31 | one row per stay: segment boundaries, completeness, channel fallback, checksum of the stay's rows |
| `data/mimic3_clinical_demo_events.csv` | 11 | vasopressor and inotrope administrations (dopamine, dobutamine) for one stay |
| `data/mimic3_monitor_limits.csv` | 73 | intervals in which a value lay beyond the alarm limit charted by staff, for one stay |
| `data/mimic3_philips_default_limits.csv` | 896 | intervals in which a value lay beyond the default adult alarm limits, 29 stays |
| `data/SHA256SUMS.txt` | | SHA-256 of every data file |

Every data file starts with comment lines (`#`) that state the source, the licence, the
meaning of each column and the assumptions. Most CSV readers can skip them, for example
`pandas.read_csv(path, comment="#")`.

## How the stays were selected

From the records of the Matched Subset, records were kept if they had 1 Hz numerics with
ABP Mean, SpO2 and heart rate, a total length of 12 to 30 hours, and a continuous segment
of at least 10 hours with at least 95 percent completeness inside that segment. A second
counts as complete when all three signals have a value. Within each record, the segment
used is the longest stretch in which incomplete seconds never run longer than 60 seconds
in a row.

Thirty stays were chosen by fixed rules to cover five patterns, six stays each: a gradual
fall of the mean arterial pressure, a sudden desaturation, combined events, stable
periods, and signal artefacts without a real event. These groups were assigned
automatically by the selection rules; clinicians have not reviewed them, and the group
labels are not part of this release. The thirty-first stay is the one record that is also
present in the Clinical Database Demo. It is included as the whole record, including a gap
in the arterial line, so that missing data stays visible.

## What was done to the values, and what was not

- Values are exactly as the monitor recorded them, including zeros and physiologically
  impossible values. Nothing was imputed, smoothed or removed.
- An empty field means that the monitor recorded no value in that second.
- `offset_s` counts whole seconds from the start of the selected segment of each record
  (`data/manifest.csv`). All files use this same time axis.
- For one record, the mean and systolic pressure come from the channel `ART` where `ABP`
  is missing. It is the same arterial line under another channel name, and the two
  channels never overlap (`map_fallback` in the manifest).
- Records keep their MIMIC names. The dates inside these names are shifted by PhysioNet's
  de-identification and do not correspond to real dates.

## Limits of this set

- **No monitor alarms.** The source databases do not contain the alarms the monitors
  actually raised. The two limit files are derived: they show where a value lay beyond a
  limit, not where a monitor alarmed. Real alarms, the limits in force and clinicians'
  assessments are exactly what the hospital dataset of this project is meant to add.
- The default-limit file assumes that the monitors ran with the manufacturer's default
  adult limits. Staff may have changed them; for the one stay with charted limits, see
  `mimic3_monitor_limits.csv`.
- Thirty-one stays from one hospital and one era are a reference set for testing tools,
  not a basis for clinical conclusions.
- This set is not a medical device and has not been clinically validated.

## Integrity

`data/SHA256SUMS.txt` holds the SHA-256 of each file (check with `sha256sum -c` inside
`data/`). The column `rows_sha256` in `data/manifest.csv` is the SHA-256 of that stay's rows
in `mimic3wdb_matched_1hz.csv.gz`, as CSV text with `\n` line endings and without the
column header.

## Licence

This set is a derived database of the sources above and is licensed under the Open Data
Commons Open Database License (ODbL) 1.0: <https://opendatacommons.org/licenses/odbl/1-0/>.
You may share and adapt it if you credit the sources and share derived databases under
the same licence.
