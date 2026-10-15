# ICU Alarm Event Log

Open tools and a data standard for building, checking, annotating and evaluating
datasets of intensive care unit (ICU) monitor alarms.

> **Status: pre-release.** This repository is being set up. The first release, v0.1,
> is planned within three months of the project start. Nothing here is a medical
> device, and nothing here has been clinically validated.

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
| Reference set derived from MIMIC-III | ICU stays from the public MIMIC-III Waveform Database Matched Subset at 1 Hz, built reproducibly with checksums, so the tools can be tried without access to hospital data. |

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
publications.

## Contributing

Issues and suggestions are welcome. This repository is a published copy of an
internal source. Accepted changes are carried over into that source and come back
with the next release, so pull requests are not merged here directly.

## Publisher

tokenova s.r.o., Prague, Czech Republic, <https://www.tokenova.eu>
