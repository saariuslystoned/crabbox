# PR 1745 supported-GUI packet — 2026-09-17 (final)

**Status: GUI route + native `0095e1a3` type + saved-port/noTools + guarded cleanup + source-final AFTER proved.** Maintainer CI/assessment remain open. Not official Autoreview, not a full-suite green claim.

`PACKET-GUI.md` and the first `public/` snapshot are the **stale 1982** draft (partial native type). The 2026-09-16 source-changing packet is **DISQUALIFIED**. The morning E2E packet stays historical.

## Revision binding

| Item | Value |
| --- | --- |
| Final head | `0095e1a32cc6379ff0d401cd5cbd4adb5fe20923` on base `f288eb7a13142fb05cbcad3c65c12e966df77044` |
| Live binary SHA-256 | `27c722172163ac237a29a687957378a069f0ce3ab411c8e119920b48d36063c9` |
| Diff `ff5d4cd5..0095e1a3` | `9b19e5b950d556748f60ebb82a823107cdd436a4` |
| Parent-stated `f288eb7a..0095e1a3` | `35c1596144bf10c9c6d8749e76ee67e33919827a` |
| Branch / PR | `work/pr1745-gui-final-20260917` / [PR 1745](https://github.com/openclaw/crabbox/pull/1745) |
| Lease / clone (released) | `cbx_12f11bb0ab75` / `cc38d3ad-2e77-4aa1-b83c-65b3cc145ea0` |
| Source / unrelated | `d1cc3c01-…` stopped; `58b49202-…` suspended; unused |
| Final RM account | `cbxfinal1745` UID 504 only |
| Helper nonce | `PR1745_FINAL_RFB_20260917_CC38` |
| Native nonces | `PR1745_NATIVE_0095E1A3_CC38` then `_SECOND_0095E1A3_OK` |
| Guest file concat | `PR1745_NATIVE_0095E1A3_CC38_SECOND_0095E1A3_OK` |

Superseded heads kept as history: `1982f4e8` (partial `_`) and `ff5d4cd5` (blank). Prior PR head `03de7896` exposed the missing-`desktop=true` refusal.

## What this packet may claim

Limits: [STATUS.md](STATUS.md).

- User-authorized owned clone only. Native acquire of `cbx_12f11bb0ab75` used the initial `03de7896` binary (not a fresh `0095e1a3` acquire). Source stayed stopped; unrelated stayed suspended.
- Disposable admins via `sysadminctl` (memory password). Final RM: **only** `cbxfinal1745` Observe+Control in normal System Settings. No kickstart, TCC DB, or autologin.
- `03de7896` correctly refused screenshot (`desktop=true` missing). `1982f4e8` is the fail-closed owned-clone guard.
- Same-session ARD-30 helper delivered `PR1745_FINAL_RFB_20260917_CC38` (independent RFB + `prlctl` captures). Earlier proof-account nonce `PR1745_RFB_GUI_20260917_1730_CC38` remains valid history.
- Native `0095e1a3` typed both nonces in a preopened blank document; independent captures, native screenshot, and post-File>Save guest file show the exact concat. No competing console input. Desktop, saved-port, and cleanup used binary `27c72217…`.
- `1982f4e8` type (trailing `_`) and `ff5d4cd5` type (blank, 33 then 23 bytes) are **failed** history, superseded by `0095e1a3`.
- Authoritative saved-port/noTools: `reuse/saved-port-final/` PASS on binary `27c72217` (`sw_vers` 26.5.2 exit 0; config 2222 / claim 22; 22 open / 2222 closed; Tools IP []; SSH `prltoolsd` absent and `=> disabled`). First `reuse/saved-port/` run is a historical parser miss (`no_tools_achieved=false`).
- Guarded exact-claim cleanup PASS: parent exact-UUID force-stop independently stopped the clone (`cleanup/parent-clone-exact-force-stop.txt`) after SSH shutdown left it running (`parent-clone-shutdown.txt`); then native `crabbox stop --id cbx_12f11bb0ab75`. Script `exact_force_stop_used=false` is the script flag, not “no force happened.” UUID/bundle/claim/keys absent; source stopped / unrelated suspended. Task Peekaboo ended; no lease SSH remained (`cleanup/task-process-cleanup.json`).
- Source-final AFTER `18:03:33Z` PASS: both HDS and all 7 source/unrelated configs match the before baseline. FileVault Off.
- Author-arranged Cursor reviews (1982 / rebase / ff5d / 0095) found no blocking findings. That is **not** acceptance.

## Open gates

1. **Maintainer CI** — `review/final-workflows.json`: four `action_required` on `0095e1a3` (CI `35255967163`, Docs UI `35255966924`, Release Check `35255967161`, Connector E2E `35255967058`). Dispatch success is not CI. `review/final-ci.json` rollup lists Dispatch + smith skipped only. Prior full `./...` was **not** green. Do not credit `1982f4e8` CI.
2. **Maintainer assessment / merge** — not granted. Official Autoreview was not invoked.

## Evidence index

| Folder | Role |
| --- | --- |
| [STATUS.md](STATUS.md) | Closed vs pending |
| [desktop/NOTES.md](desktop/NOTES.md) | GUI, helper nonce, 1982/ff5d failures, 0095 pass |
| [readiness/NOTES.md](readiness/NOTES.md) | `03de7896` acquire / claim / clone-only Node |
| [reuse/NOTES.md](reuse/NOTES.md) | Authoritative saved-port-final; historical first run |
| [cleanup/](cleanup/guarded-cleanup.json) | Guarded release; parent force-stop; task-bridge end |
| [source/NOTES.md](source/NOTES.md) | Before hashes; source-final AFTER PASS |
| [review/NOTES.md](review/NOTES.md) | Author-arranged reviews ≠ acceptance; four held workflows |
| [test/author-arranged-local-verification.md](test/author-arranged-local-verification.md) | Scoped tests; full suite not green |
| [MANIFEST.sha256](MANIFEST.sha256) | SHA-256 of every published file except itself |

## Sanitization

| Token | Replaces |
| --- | --- |
| `$TASK_ROOT` | absolute GUI-task work-root prefix |
| `$E2E_ROOT` | prior E2E remote root (isolated config / state / keys) |
| `$SOURCE_ROOT` | source VM bundle prefix |
| `$HOST_HOME` | Parallels-host home prefix outside `$TASK_ROOT` / `$E2E_ROOT` / `$SOURCE_ROOT` |
| `$CHECKOUT` | final implementation worktree |
| `$CHECKOUT_E2E` | prior 1982 implementation worktree |
| `$PROVIDER_KEY` | non-public provider-key label |
| `$GUEST_IP` | clone IPv4 from the acquire receipt |
| `$PROOF_HOST` | durable proof-host identity |
| `$HOST_USER` | durable host account |
| `$SSH_CONTROL_PATH` | SSH multiplex control path |
| `unrelated-vm` | unrelated VM display name / bundle name |

Kept: lease IDs, VM UUIDs, SHAs/digests, times, statuses, disposable guests `cbxgui1745` / `cbxproof1745` / `cbxfinal1745`, stock SSH user `parallels-02`, Crabbox run metadata, `naprivs`, nonce strings.

Excluded: `host-*`, generic `current.png`, password-field / recovery / onboarding / login / setup captures, failed black-frame PNGs, ff5d blank frames, credentials, SSH key bodies, agent/session IDs, helper source, signed tracker query strings.

Visible desktop proof is the named independent frames, not helper `input-effect-candidate` / CLI byte count. The 2s settle is a documented delay, not protocol or glyph proof. Maintainer acceptance remains open.
