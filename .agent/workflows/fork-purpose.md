---
description: Documents the purpose and scope of our ruff fork modifications for APK integration
---

# Ruff Fork Purpose

This fork of [astral-sh/ruff](https://github.com/astral-sh/ruff) exists solely to
**widen the API visibility** of the `ty_server` crate so that the `apk` project
can embed ty's language server as a library rather than a subprocess.

## What We Changed

ALL changes are visibility-only (`pub(crate)` → `pub`, `pub(super)` → `pub`,
`pub(in crate::...)` → `pub`, `mod` → `pub mod`). **Zero logic modifications.**

### Modified Crate: `crates/ty_server/`

| File | Changes |
|------|---------|
| `src/lib.rs` | Made `session`, `server`, `document`, `system`, `capabilities`, `db`, `logging` modules `pub`; re-exported `Session` as `pub` |
| `src/session.rs` | Made `Session`, `ProjectState`, `SessionSnapshot`, `DocumentSnapshot` structs `pub`; all `pub(crate) fn` → `pub fn`; made `client`, `index`, `options`, `request_queue`, `settings` submodules `pub` |
| `src/server.rs` | Made `api`, `schedule`, `main_loop`, `lazy_work_done_progress` modules `pub`; widened re-exports |
| `src/server/api.rs` | All `pub(crate)` and `pub(super)` → `pub` |
| `src/server/api/traits.rs` | All handler traits made `pub` |
| `src/server/api/requests.rs` | All handler re-exports made `pub` |
| `src/server/api/requests/*.rs` | All handler structs made `pub` |
| `src/server/api/notifications.rs` | All re-exports made `pub` |
| `src/server/api/notifications/*.rs` | All notification handler structs made `pub` |
| `src/server/schedule.rs` | `Scheduler`, re-exports made `pub`; submodules made `pub` |
| `src/server/schedule/task.rs` | `BackgroundSchedule`, `Task` made `pub` |
| `src/document/*.rs` | All traits, types, methods made `pub` |
| `src/capabilities.rs` | All items made `pub` |
| `src/system.rs` | All items made `pub` |
| `src/session/client.rs` | All items made `pub` |
| `src/session/index.rs` | All items made `pub` |

## Why Not Upstream This?

These changes expose internal APIs that upstream may not want to stabilize.
We maintain this as a thin fork layer with minimal merge conflict risk.

## Merge Strategy

See `merge-upstream.md` workflow for automated upstream sync.
