# tla-core

The **unified data repo** for TLA Stats — replacing the one-repo-per-cron
`*-data_2026` sprawl. Structured **module → product → files**.

```
{module}/{product}/...
 fuel   /snapshots/   ← pilot SNAPSHOT module (live, hourly)
 flows  /events/      ← pilot EVENT module (tla-flows; deploy pending)
```

- **module** = a cron/domain (mirrors `cron-scripts/{module}/`).
- **product** = the data shape: `snapshots/` (state at a time) or `events/`
  (append-only stream). A module may have more than one.
- **files:** `heartbeat.json` + `index.json` always; `cursor.json` for event
  products; `current.json` + `daily/` + `hourly/` for snapshots; year/month
  partitions (`{YYYY}/{MM}/{DD}.jsonl` events, `{YYYY}/{rollup}/{YYYY}.csv` snapshots).

**Year rollover = a new folder, never a new repo.** 2027 is just
`{module}/{product}/2027/`.

**Canonical convention, schemas, and the migration checklist live in
`website-adao-core/TLA-CORE-STORAGE-DESIGN.md`** — this README is the front door;
that doc is the source of truth. Don't duplicate the convention here; point to it.

System Health monitors each module at
`tla-core/main/{module}/{product}/heartbeat.json`.
