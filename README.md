# ZeroHost — Postmortems

Official postmortems for incidents affecting ZeroHost (dashboard, Hydrodactyl, homepage, and related infrastructure).

Each postmortem documents what happened, the root cause, the impact on users, and the corrective actions taken to prevent recurrence.

## Purpose

Better Stack (our status page provider) doesn't support built-in postmortems. This repository fills that gap, giving us a transparent, version-controlled record of incidents for both internal tracking and public accountability.

## Structure

Postmortems are organized by date and stored as Markdown files:

```
/postmortems/YYYY-MM-DD-short-title.md
```

## Postmortem template

Each postmortem follows this structure:

- **Summary** — brief description of the incident
- **Impact** — services and users affected, duration
- **Timeline** — key events in chronological order (UTC)
- **Root Cause** — technical explanation of what went wrong
- **Resolution** — how the incident was resolved
- **Corrective Actions** — steps taken or planned to prevent recurrence

See [`TEMPLATE.md`](./TEMPLATE.md) to start a new postmortem.

## Status page

Live incident tracking: [status.zero-host.org](https://status.zero-host.org)

## License

ZHSL
