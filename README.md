# ICU Alarm Event Log

Open tools and a data standard for building, checking, annotating and evaluating
datasets of intensive care unit (ICU) monitor alarms.

> **Status: release v0.0.1 (15 October 2026), the MIMIC-III reference set.** 31 ICU stays
> at 1 Hz with derived alarm-limit context, a data card and checksums. The data standard
> with its validator (v0.1) is planned within three months of the project start. Nothing
> here is a medical device, and nothing here has been clinically validated.

## Why this exists

ICU patient monitors raise many alarms, and most of them call for no action at the
bedside. Staff learn to tune them out, which is known as alarm fatigue. Methods
that sort alarms by clinical relevance need data to learn from and to be compared
on. Such data has to show, on one time axis:

- the vital signs,
- the alarms the monitor actually raised,
- the alarm limits in force at that moment,
- the drugs given,
- and the clinicians' assessment of each alarm.

In the public ICU datasets we have reviewed, these pieces are not available
together. This repository provides the open building blocks for such a dataset.

## What will be published here

| Component | Purpose |
| :--- | :--- |
| **ICU Alarm Event Log**, a data standard with a validator | One time axis for vital signs, monitor alarms, limits in force, drugs and clinician assessment. The validator checks that a file follows the standard. |
| ETL for monitor exports | Converts exports from ICU monitoring systems into the standard. |
| Annotation tool and protocol | Lets clinical staff mark alarms and interventions on a timeline, with a written protocol for consistent annotation. |
| Benchmark with pre-specified metrics | Evaluates any alarm system from a simple CSV file of alarm times, so different approaches can be compared on equal terms. |
| Reference set derived from MIMIC-III (**available now**, `data/`, see `DATA_CARD.md`) | 31 ICU stays from the open-access MIMIC-III Waveform Database Matched Subset at 1 Hz, with checksums, so the tools can be tried without access to hospital data. The scripts that build it follow with v0.1. |

## Quick start

```python
import pandas as pd

vitals = pd.read_csv("data/mimic3wdb_matched_1hz_sys.csv.gz", comment="#")
limits = pd.read_csv("data/mimic3_philips_default_limits.csv", comment="#")
print(vitals.groupby("record").size().describe())
```

Check the files with `sha256sum -c SHA256SUMS.txt` inside `data/`.

## What will never be published here

- **No patient data from partner hospitals.** A hospital dataset is released
  separately, under controlled access only: credentialed researchers and a data
  use agreement, in the way MIMIC-III and HiRID are distributed. That happens only
  with each hospital's approval.
- **No proprietary decision logic.** The alarm-decision software of the publisher
  is not part of this repository.

## Licences

| Content | Licence |
| :--- | :--- |
| Code | Apache License 2.0 (see `LICENSE`) |
| Documentation | Creative Commons Attribution 4.0 International (CC BY 4.0) |
| Data derived from MIMIC-III | Open Database License (ODbL) 1.0, following the licence of the source data |

Anyone using the reference set must also cite the original MIMIC-III and PhysioNet
publications (see `DATA_CARD.md` and `CITATION.cff`).

## Contributing

Issues and suggestions are welcome. This repository is a published copy of an
internal source. Accepted changes are carried over into that source and come back
with the next release, so pull requests are not merged here directly.

## Publisher

tokenova s.r.o., Prague, Czech Republic, <https://www.tokenova.eu>
