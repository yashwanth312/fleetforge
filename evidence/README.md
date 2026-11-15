# Evidence

One folder per phase: `evidence/phase-N-slug/`. Each folder holds dated entries named `YYYY-MM-DDTHHMM-slug.md`, with screenshots (when used) sharing the same prefix. A family of related tests can be one file that grows by section.

Each entry contains:

- Title, and the date with time zone (for example `2026-11-15 11:06 (America/Chicago, UTC-06:00)`).
- Host and versions.
- Sources: links to screenshots or saved reports.
- Method: the exact commands, and how each time, size or score was measured.
- Results in tables, with values transcribed into the text.
- What changed on the way: only failures that changed the plan.
- Prediction check: what was predicted before the run versus what was measured, including misses.
- Caveats: one short line.

Entries are added when a step is verified with recorded output. A figure appears in the README results table only after an entry here records how it was measured.
