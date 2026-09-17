# Known incomplete status

Packet time: 2026-09-17. Scope: sanitized evidence from the fresh owned-clone GUI route through native `0095e1a3`. No live VM, SSH, GitHub, or repo edits were performed while building this directory. Maintainer acceptance remains **OPEN**. Source-final AFTER is **closed** (`18:03:33Z` PASS).

## Closed in this packet

| Item | Result | Evidence |
| --- | --- | --- |
| Final head + binary | `0095e1a3` / `27c72217…` on base `f288eb7a` | review identity + live pin |
| Author-arranged Cursor reviews | no blocking findings on `1982f4e8`, rebase, `ff5d4cd5`, `0095e1a3` | `review/*.md` |
| Native acquire (`03de7896` binary) | exit 0; lease `cbx_12f11bb0ab75`; VM `cc38d3ad-…` | `readiness/acquire-ssh.txt` |
| Exact claim before GUI | `sshPort=22`, host `local`, claimed `2026-09-17T16:59:04Z` | `readiness/claim-before-gui.json` |
| Clone-only Node/npm | official `22.23.1` / npm `10.9.8` | `readiness/clone-node-prerequisite.txt` |
| Source + unrelated inventory | source stopped; unrelated suspended | `source/inventory-before.json` |
| Source disk before-hash | both HDS; morning baseline | `source/disk-before.txt` |
| Configs after GUI / final GUI | all 7 match before | `source/config-before.sha256`, `config-after-gui.sha256`, `config-after-final-gui.sha256` |
| Native account create | `cbxgui1745` 502, `cbxproof1745` 503, `cbxfinal1745` 504; `sysadminctl`; kickstart not invoked | desktop helper transcripts |
| Old blocker on parent binary | `03de7896` screenshot exit 2: missing `desktop=true` | `desktop/gui-helper.txt` |
| Final System Settings RM | RM On; **only** `cbxfinal1745` Observe+Control | `desktop/final-account-*.png`, `final-account-permissions.txt` |
| Helper ARD-30 nonce | `PR1745_FINAL_RFB_20260917_CC38` in RFB **and** independent `prlctl` | `desktop/final-account-same-session-*.png` |
| Prior proof-account nonce | `PR1745_RFB_GUI_20260917_1730_CC38` still valid history | `desktop/proof-account-rfb*.png` |
| Native `0095e1a3` type + screenshot | both nonces visible; screenshot + guest file exact concat | `desktop/native-0095-*.png`, `native-0095-file-independent.txt` |
| Saved-port + noTools (authoritative) | PASS on `0095e1a3` / `27c72217…`; `no_tools_achieved=true` | `reuse/saved-port-final/` |
| Parent exact force-stop | clone independently stopped before script cleanup | `cleanup/parent-clone-shutdown.txt`, `parent-clone-exact-force-stop.txt` |
| Guarded exact-claim cleanup | PASS; clone/claim/keys/bundle absent; source stopped / unrelated suspended | `cleanup/guarded-cleanup.json` |
| Task-process cleanup | Peekaboo `54520` ended; clone app absent; no lease SSH | `cleanup/task-process-cleanup.json` |
| Source-final AFTER | PASS `18:03:33Z`; 2 HDS + 7 configs match before | `source/source-final/` |
| FileVault on clone | Off | `desktop/final-account-permissions.txt` |

## Failed history (retained, superseded)

| Item | State | Do not claim |
| --- | --- | --- |
| Other-user / black-frame RFB | **FAILED**. Probe exit 0 + `input-effect-candidate` was a false positive. | desktop success from that frame |
| Native `1982f4e8` type `_CRABBOX_1982F4E8_1736` | **PARTIAL**. Helper bytes=22 exit 0; only a trailing `_` landed. | native type success on 1982 |
| Native `ff5d4cd5` type | **FAILED**. 33 bytes then 23 bytes; blank document. Frames not published. | native type success on ff5d |
| First saved-port run | reuse PASS but `no_tools_achieved=false` (parser wanted `true`/`false`; macOS 26 prints `=> disabled`) | that run as noTools |

## Open / pending

| Item | State | Do not claim |
| --- | --- | --- |
| Spotlight app launch | **NOT PROVEN**. Documents were preopened. | RFB launched TextEdit |
| Current-head maintainer CI | `0095e1a3` `MERGEABLE`/`BLOCKED`; four workflows `action_required`; Dispatch success is not CI. Prior full suite not green. | CI green / suite green |
| Maintainer assessment | **NOT GRANTED** | merge readiness |
| Official Autoreview | **NOT RUN** | official review |

## Disqualified

The 2026-09-16 source-changing proof is **DISQUALIFIED**. Other-host source `b98e38c2-...` was historically modified and was not operated this task.

## Stated, not independently re-queried here

- User authorized only the owned clone. Source `d1cc3c01-...` and unrelated `58b49202-...` were not operated.
- Final console account is `cbxfinal1745` after a normal GUI login. No TCC DB edit, autologin, or kickstart.
- Independent captures show helper nonce `PR1745_FINAL_RFB_20260917_CC38` and both native `0095e1a3` nonces in the named frames.
- Scoped tests for 1982 / rebase / ff5d / 0095 passed on implementation agents. Author-arranged reviews did not rerun tests. Prior `03de7896` full `./...` was not green. Full suite was not rerun on `0095e1a3`.
- `0095e1a3` was force-with-lease pushed over `1982f4e8`. Proof pin stays `0095e1a3` on `f288eb7a` even though GitHub `baseRefOid` later shows `276a9047` (unrelated provider-cfg diagnostics; MERGEABLE).
- Native acquire is the `03de7896` warmup. Desktop type, saved-port/noTools, and guarded cleanup are the `0095e1a3` / `27c72217…` validation. Do not say the acquire ran at the final head.
- Script `exact_force_stop_used=false` means the cleanup script did not issue `prlctl stop --kill`. The parent already had.
