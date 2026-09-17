# Known incomplete status

Packet time: 2026-09-17. Scope: sanitized evidence for a **fresh** native `0095e1a3` no-Tools acquire and implemented native Stop. No live VM, SSH, GitHub, or repo edits were performed while building this directory. Maintainer implementation / evidence assessment remains **OPEN**, including capability-gate design. Fixture-after hash, exact fixture delete, original-source final AFTER, and closeout are **closed**.

## Closed in this packet

| Item | Result | Evidence |
| --- | --- | --- |
| Final head + binary | `0095e1a3` / `27c72217…` unchanged | parent pin + warmup receipt |
| Fresh native warmup | `20:20:50Z` 18.358s exit 0; lease `cbx_c19f90406ff5`; VM `65f55c4d-…` | `acquire/native-warmup.excerpt.json`, `warmup.stderr.txt` |
| Discovery / bootstrap | stderr `discovery=dhcp-mac bootstrap=ssh`; claim `ip_source=dhcp-mac` | `acquire/warmup.stderr.txt`, `claim.json` |
| New identity | MAC `001c426e3c3b` ≠ fixture `001c42a811cf`; empty claims/state before | `acquire/fresh-native-verdict.json` |
| Matching DHCP row | `10.211.55.14` ↔ `001c426e3c3b` | `acquire/matching-dhcp-row.txt` |
| No-Tools after acquire | Tools IPs `[]`; exec 255 exact platform message; service absent/disabled | `acquire/NOTES.md` |
| Lease-key audit | uid 501; **observed** keys `Gdd…` + `vBa6…` (audit did not install); markers present; `sudo_n=ok`; Node 24.19 | `acquire/lease-ssh-audit.txt` |
| Explicit follow-on `run` | `20:22:25Z` exit 0; `sw_vers` 26.5.2 | `acquire/native-run-explicit-bootstrap.json` |
| Parent acquire verdict | PASS on evidence hashes; original helper FAIL unchanged | `acquire/fresh-native-verdict.json` |
| Native Stop from RUNNING | `20:23:10Z` exit 0; no parent pre-kill / guest halt; all absence PASS | `stop/native-stop.json` |
| Stop semantics | implemented `--kill`+delete; **not** graceful | `stop/NOTES.md`, `research/shutdown.md` |
| Hydration warning | nonfatal Actions stop-marker 255; no Actions requested | `stop/native-stop.json` |
| Fixture operator prereqs | SSH + bootstrap key + sudo + Node 24.19; Tools disable/restart | `fixture/prepare.excerpt.json` |
| Frozen audit (parent) | PASS: markers absent, 1 key, Tools unavailable, `sudo_n=ok` | `fixture/frozen-audit-parent.json` |
| Fixture freeze-before | `20:19:44Z` PASS; 2 HDS + 6 configs | `fixture/fixture-frozen-before.json` |
| Fixture freeze-after | `20:24:57Z` PASS; all 8 equal freeze-before | `fixture/fixture-frozen-after.json`, `fixture-frozen-verdict.json` |
| Exact fixture delete | `20:27:49Z` PASS; UUID/bundle absent; source stopped / unrelated suspended | `fixture/fixture-delete.json` |
| Original source-before | `19:59:06Z` PASS vs GUI baseline; 2 HDS + 7 configs; source stopped / unrelated suspended | `source/` |
| Original source-final AFTER | `20:30:45Z` PASS; same 2 HDS + 7 configs; source stopped / unrelated suspended | `source/source-final-comparison.json` |
| Closeout | `20:31:27Z` PASS; only source stopped / unrelated suspended; fixture+lease bundles/keys/known_hosts absent; `remaining_task_processes=[]` | `source/final-closeout.json` |
| Parent-stated CI on this head | `20:28:52Z` query: `CLEAN` / `MERGEABLE`; 21 SUCCESS, 2 SKIPPED, 0 FAILURE | `ci/current-pr-state.json` |

## Failed / incomplete helper receipts (retained, not product FAILs)

| Item | State | Do not claim |
| --- | --- | --- |
| Helper `fresh-acquire.json` | **FAIL** (classifier + omitted follow-on flag) | that the native acquire failed |
| Helper frozen-audit | **FAIL** (overly-narrow exec-unavailable whitelist) | that Tools were available |
| Helper argv `--` | live `prlctl exec` rejects `--`; parent used native argv | product exec uses `--` |
| Helper `commentNoReboot` | `# No reboot` comments false-refused helper execute | that guest reboot was required for SSH/sudo/Node |
| Initial helper follow-on `run` | omitted `--parallels-bootstrap-key`; timed out (`repo_config` ignores host key) | acquire failure; this is a designed config-trust boundary |
| Helper `tools_metadata.no_tools_achieved` | helper `false` on unknown load/disable fields | that Tools IPs or exec were available |
| Helper fixture-delete | refused verdict shape, then `AttributeError` missing `run_exact_fixture_delete` | that parent delete failed |

## Open / pending

| Item | State | Do not claim |
| --- | --- | --- |
| Maintainer assessment | Pete implementation / evidence assessment, including capability-gate design; **not** closed by this packet | merge readiness |
| Capability-gate policy | still open inside that remaining assessment | that the generic desktop/capability exception is accepted |
| Graceful ACPI / `shutdown -h now` | **NOT PROVEN**; our recommendation is optional follow-up, not a 1745 gate | graceful native Stop |

Official Autoreview was **not run**. That is a fact, not a required gate listed here.

## Stated, not independently re-queried here

- Parent gh query `20:28:52Z` on unchanged head `0095e1a3` is `CLEAN` / 21 SUCCESS / 2 SKIPPED / 0 FAILURE (`ci/current-pr-state.json`). This packet copied that receipt and does not request CI approval.
- User authorized only the disposable fixture and the exact new lease. Original source `d1cc…` and unrelated `58b492…` were not operated. Other-host `b98e38c2-…` was not operated.
- Doctor on the pinned binary was `mutation=false` (parent). Raw doctor text is omitted (private paths; not needed).
- Node 24.19 was an **operator image prerequisite** because native macOS `sshReady` requires `node --version` and `npm --version`. Do not publish the research claim that Node is unused.
- Guest `sudo_n=ok` on both frozen and post-acquire audits. Do not publish helper FIX notes that say sudo was missing.
- Fresh acquire assumes a **prepared SSH image** (SSH, bootstrap key, sudo, Node), not an unprovisioned OS.
