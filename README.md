# 📊 SignalDesk Weekly Check

> **A trust-first readout for messy AI-workflow usage data — quarantines rows it can't trust before reporting any headline number, then shows the answer without the quarantine.**

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org)
[![pandas](https://img.shields.io/badge/pandas-numpy_only-150458.svg)](https://pandas.pydata.org)
[![Track A](https://img.shields.io/badge/challenge-Track_A-orange.svg)](https://github.com/vyuan2037/ds-intern-challenge)

---

## Overview

**Track A** (fictional domain packet). One script, one question, for whoever writes this area's weekly update: *can we trust this export, and did the Aug 4 prompt change help?* Run it before the numbers go in a doc — it's not a dashboard.

```
python signaldesk_check.py --csv sample-data/product_usage_events.csv
```

One-screen readout: hazard scan → trusted-rows summary → prompt-change sensitivity test → confidence check → known blind spots.

---

## Key Findings

- **The verdict depends on 3 rows.** Acceptance is **+4.2pts** post-change as exported, **−2.7pts** after removing one duplicate and two anomalies (32% of post-change sessions). The script reports *unresolved* rather than picking a side.
- **`median_confidence` is the metric to trust least.** It rose every day in every workflow, and peaked at 0.91 on the week's worst day (27% acceptance, 71% flag rate).

---

## Design Decisions

| Decision | Why |
| --- | --- |
| Anomalies found structurally (>2× / <0.5× series median), not by matching `notes` | A check that needs someone to type "demo account" fails silently. Verified with `notes` blanked: all hazards still caught |
| Session-weighted rates, never mean-of-row-rates | A 6-session day shouldn't count like a 140-session one |
| Quarantine ≠ delete | Out of headlines, stress-tested in the sensitivity table, kept as evidence in the confidence check |
| Pre = Aug 1–3, post = Aug 4–7 | Read from `notes`; `--change-date` overrides |

---

## Limitations

- **Thresholds are untuned.** 2.0×, 0.5×, 30 sessions are round numbers.
- **Weak sample.** Spearman on 6–7 points per series is a lead, not a finding.
- **Missing data stays missing.** Aug 5 and Aug 7 lack series even after cleaning — the script warns, it doesn't impute.

---

## Future Work

- Ask whether demo traffic is tagged upstream  makes this hazard class a join, not a heuristic
- Sessions split by user, so "runs" become "people"
- Untangle the Aug 7 review-policy change from the prompt change currently confounded
