# PR 1745 fresh no-Tools acquire packet — 2026-09-17

**Status: fresh native `0095e1a3` acquire proved (`discovery=dhcp-mac`, `bootstrap=ssh`); native Stop from independently RUNNING proved as longstanding force/delete, not graceful; fixture-after hash matched freeze-before; exact fixture delete PASS; original-source final AFTER `20:30:45Z` PASS; closeout `20:31:27Z` PASS.** Helper classifier FAILs retained. Maintainer implementation/evidence assessment, including capability-gate design, remains open. Official Autoreview was not run. Not a merge claim.

Prior GUI / type / saved-port packet stays valid and separate:
[saariuslystoned/crabbox `8de05c02640c6803955f665eeb6cfde935f94a75` / `proof/pr1745-gui-20260917`](https://github.com/saariuslystoned/crabbox/tree/8de05c02640c6803955f665eeb6cfde935f94a75/proof/pr1745-gui-20260917).

## Revision binding

| Item | Value |
| --- | --- |
| Head | `0095e1a32cc6379ff0d401cd5cbd4adb5fe20923` |
| Live binary SHA-256 | `27c722172163ac237a29a687957378a069f0ce3ab411c8e119920b48d36063c9` (unchanged) |
| Branch / PR | [PR 1745](https://github.com/openclaw/crabbox/pull/1745) |
| Fresh lease / clone (released) | `cbx_c19f90406ff5` / `65f55c4d-2f6e-42eb-bc40-8910993486f7` |
| Fresh MAC | `001c426e3c3b` (fixture `001c42a811cf`) |
| Disposable fixture | `5b2f2234-a4d2-4600-a7a3-a9c618599246` |
| Original source | `d1cc3c01-c6a0-43a1-82c0-61b841f8f636` stopped; **untouched** |
| Unrelated | `58b49202-a6de-4e24-ba8f-c1890c778535` suspended; unused |
| Other-host source | `b98e38c2-…` not operated |

This packet answers Pete's 19:06Z remaining coverage gap: a **fresh** no-Tools acquire (`ip_source=dhcp-mac`, not a reuse of a Tools-era claim). It does **not** replace the GUI packet. The fresh path assumes a **prepared SSH image** (SSH, bootstrap key, sudo, Node), not an unprovisioned OS.

## What this packet may claim

Limits: [STATUS.md](STATUS.md).

- Native `0095e1a3` / `27c72217…` warmup at `20:20:50Z`, 18.358s, exit 0. Actual stderr: `discovery=dhcp-mac bootstrap=ssh`. Claim `ip_source=dhcp-mac`. Empty claims/state/install before acquire. New UUID + MAC. Matching DHCP row for `001c426e3c3b`.
- Disposable fixture `5b2f…` was operator-prepared only (SSH, task bootstrap key, NOPASSWD sudo, official Node 24.19 / npm 11.17). Not native Crabbox bootstrap. Tools disabled, then restart. Frozen audit: all product markers absent; one authorized key (`Gdd…`); `sudo_n=ok`; service absent/disabled.
- After acquire: Tools IPs `[]`; `prlctl exec` exit 255 with the exact platform guest-session message; `prltoolsd` absent + `=> disabled`. Native bootstrap **installed** the new lease key. A later authenticated lease-key SSH audit as uid 501 **observed** fingerprint `vBa6…` beside bootstrap `Gdd…`; the audit did not install it. Native product markers present (`crabbox-ready`, `/var/lib/crabbox/bootstrapped`, `$GUEST_HOME/crabbox-work`). `/var/db/crabbox/bootstrapped` stays absent (0095 does not write it).
- Initial **ACQUIRE included** `--parallels-bootstrap-key` and **passed**. Do not call the acquire failed.
- Helper `fresh-acquire.json` remains `FAIL`. Parent `fresh-native-verdict.json` is a separate assessment on evidence hashes. Helper FAILs: argv `--`, `commentNoReboot`, overly-narrow unavailable-message whitelist, and the initial follow-on `run` that omitted the trusted bootstrap flag. That `run` timed out because `CRABBOX_CONFIG` inside cwd is `repo_config` and therefore ignores the host-side bootstrap key — **designed boundary**, not an acquire failure. Explicit-flag `run` at `20:22:25Z` exit 0, `sw_vers` 26.5.2.
- Native `crabbox stop` from independently RUNNING at `20:23:10Z`, no parent pre-kill, no guest halt: exit 0; UUID / bundle / claim / key dir / lease metadata absent. Nonfatal GitHub Actions hydration-stop-marker 255 warning retained (no Actions requested). Native Stop is longstanding `--kill`+delete, **not** graceful ACPI.
- Original source lineage and frozen-fixture immutability are recorded **separately**. Source-before `19:59:06Z` PASS vs the prior GUI baseline (2 HDS + 7 configs). Source-final AFTER `20:30:45Z` PASS (same 2 HDS + 7 configs). Fixture freeze-before `20:19:44Z` and freeze-after `20:24:57Z` PASS (same 2 HDS + 6 configs). Exact fixture delete `20:27:49Z` PASS. Closeout `20:31:27Z` PASS: both disposables gone; `remaining_task_processes=[]`.

## Open (not invented)

1. Maintainer implementation / evidence assessment, including capability-gate design (unchanged from Pete's 19:06Z note).
2. Merge — not granted and not requested here.

Official Autoreview was not run. That is not listed as a required gate.

## Evidence index

| Folder | Role |
| --- | --- |
| [STATUS.md](STATUS.md) | Closed vs pending |
| [acquire/](acquire/NOTES.md) | Native warmup, claim, DHCP, lease-key audit, explicit `run` |
| [stop/](stop/NOTES.md) | Native Stop from RUNNING |
| [fixture/](fixture/NOTES.md) | Operator prereqs, frozen audit, freeze-before/after, exact delete |
| [source/](source/NOTES.md) | Original `d1cc…` lineage; source-final AFTER + closeout PASS |
| [ci/current-pr-state.json](ci/current-pr-state.json) | Parent gh query `20:28:52Z`: CLEAN, 21 SUCCESS, 2 SKIPPED, 0 FAILURE |
| [research/shutdown.md](research/shutdown.md) | Graceful Stop is our recommended optional follow-up, not a 1745 gate |
| [helper/NOTES.md](helper/NOTES.md) | Retained helper FAILs; designed `repo_config` boundary |
| [MANIFEST.sha256](MANIFEST.sha256) | SHA-256 of every published file except itself |

## Sanitization

All files under `public/` are **sanitized excerpts**. They do not claim a raw byte match with local unsanitized evidence.

Embedded `evidence_sha256` / `original_*` / `source_final_sha256` / `original_record_sha256` values are SHA-256 of the **original unsanitized local bytes**. [MANIFEST.sha256](MANIFEST.sha256) hashes the **sanitized published files**. Those two families need not match after paths are redacted.

| Token | Replaces |
| --- | --- |
| `$FRESH_ROOT` | fresh-task work-root prefix |
| `$SOURCE_BUNDLE` | original source VM bundle |
| `$UNRELATED_BUNDLE` | unrelated VM bundle |
| `$TASK_SECRETS` | task bootstrap-key directory (public fingerprint only) |
| `$GUEST_HOME` | stock guest home (`parallels-02`) |
| `$PINNED_CBX` | pinned `0095e1a3` binary path |
| `unrelated-vm` | unrelated VM display name |
| `<omitted-host-ssh-key>` | host default SSH key path from `config show` |
| `<omitted>` | remaining private/home/host path |

Kept: lease IDs, VM UUIDs, MACs, public SSH fingerprints, SHA-256 digests, times, statuses, stock SSH user `parallels-02`, Parallels shared-network IPs needed for DHCP-MAC correlation, CI check names/conclusions/job IDs.

Excluded: private key bodies, host/home paths, all-providers `config show` dump (labeled excerpt only), helper FIX notes that falsely claim `sudo` missing, research that claims Node is unused, helper source.

Parent-stated CI on `0095e1a3` is already green (`CLEAN`; 21 SUCCESS, 2 SKIPPED, 0 FAILURE). This packet does not request CI approval.
