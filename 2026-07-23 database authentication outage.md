# Postmortem: Database Authentication Outage & Server Creation Failure

**Date:** July 23, 2026
**Duration:** ~05:30 – 13:19 (Paris time), ~7h49 total
**Status:** Resolved

## Summary

An attempt to fix a server creation issue on the preprod dashboard led to unrelated database and Wings configuration changes. Login and account creation on both preprod and production went down after 06:01 due to a previously rotated MariaDB root password that had not been documented, blocking the team from making further fixes. Once database access was restored, server creation remained broken due to a Wings permissions issue unrelated to the original changes made.

## Impact

| Service | Status | Duration |
|---|---|---|
| Server creation | Down | ~05:30 – 13:19 (~7h49) |
| Dashboard login (preprod & prod) | Down | ~06:01 – 13:15 (~7h14) |
| Account creation | Down | ~06:01 – 13:15 (~7h14) |

## Timeline (Paris time)

- **~05:30** — Server creation starts failing. Team begins investigating, suspecting the issue originates in the preprod dashboard.
- **05:30 – 06:01** — Patches applied to the preprod dashboard in an attempt to fix server creation.
- **06:01** — Login and account creation break on both preprod and production dashboards.
- **06:01 – 13:00** — Team unable to connect to MariaDB to fix the issue: the root user's password had been rotated a few weeks earlier as a planned security measure, but the change was never documented or shared, leaving no known working credentials.
- **~13:00** — Root access to MariaDB recovered by resetting the root password.
- **~13:00 – 13:15** — Password updated for the `zerohost` database user (used by the ZeroHost dashboard) and for the root user; new credentials updated in the `.env` files.
- **13:15** — Services restarted; dashboard login and account creation restored on preprod and production.
- **13:15 – 13:19** — Server creation still failing. Investigation of the Wings configuration reveals that server installation was writing to the `tmp` directory, which Wings did not have write permissions on.
- **13:19** — Wings configuration updated to stop using `tmp` for server installation. Server creation fully restored.

## Root Cause

Two independent issues combined into this incident:

1. **Undocumented credential rotation:** The MariaDB root password had been changed weeks prior as a planned security measure, but the new password was not recorded or shared with the team. This left no way to access the database once login issues appeared, turning a contained problem into a multi-hour outage.
2. **Wings permissions misconfiguration:** The actual cause of the original server creation failure was unrelated to the dashboard — Wings was configured to use the `tmp` directory for server installation, but lacked write permissions on that directory. Initial troubleshooting incorrectly focused on the dashboard, delaying identification of the real cause.

## Resolution

- MariaDB root password reset, restoring database access.
- Password rotated for both the `root` and `zerohost` database users, with new credentials properly saved in the environment files.
- Wings configuration updated to remove `tmp` as the server installation directory, resolving the underlying permissions issue.
