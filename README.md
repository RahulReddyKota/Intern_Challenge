# SignalDesk weekly check

**Track A.** Pandas and numpy only; prints a one-screen readout.

```
python signaldesk_check.py --csv sample-data/product_usage_events.csv
```

## What it is, and for whom

One script, one question, for whoever writes this area's weekly update: *can we
trust this export, and did the Aug 4 prompt change help?* Run it before the
numbers go in a doc; it does not replace a dashboard.

Everything follows from one choice: quarantine untrusted rows **before**
reporting any headline number, then show the answer without the quarantine.

## What it found

Acceptance is **+4.2pts** post-change as exported, **−2.7pts** after removing one
duplicate and two anomalous rows. Three rows of 41 — 32% of post-change
sessions — decide the sign, so the script reports *unresolved*.

`median_confidence` is the metric to trust least: it rose every day in every
workflow, and peaked at 0.91 on the worst day of the week (27% acceptance, 71%
flag rate).

## Assumptions

- Pre = Aug 1–3, post = Aug 4–7, read from `notes`; overridable via flag.
- Rates are session-weighted, never a mean of per-row rates.
- Anomalies are found structurally (>2× or <0.5× a series' own median), not by
  matching `notes` — a check needing someone to type "demo account" fails
  silently. Verified with `notes` blanked: all still caught.
- Quarantine ≠ delete. Quarantined rows still count as evidence in section 4.

## Known issues

- Thresholds (2.0×, 0.5×, 30 sessions) are round numbers, untuned.
- Spearman on 6–7 points is weak. Section 4's manual-vs-automated split is a
  lead, not a finding.
- Aug 5 and Aug 7 are missing series even after cleaning. It warns, not imputes.

## Next

1. Ask whether demo traffic is tagged upstream. If so, this hazard class becomes
   a join, not a heuristic.
2. Get sessions split by user, so "runs" can become "people."
3. Untangle the Aug 7 policy change from the prompt change. They are confounded
   and no cleaning fixes that.
