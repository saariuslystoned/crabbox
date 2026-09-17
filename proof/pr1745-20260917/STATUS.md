# Known incomplete status

Packet time: 2026-09-17. Scope: sanitized evidence from the completed run. No live VM, SSH, GitHub, or repo edits were performed while building this directory. Overall acceptance remains **INCOMPLETE**.

## Closed in this packet

| Item | Result | Evidence |
| --- | --- | --- |
| Final head + binary identity | SHA `03de7896...` / binary `7db4eea831...` / candidate `17fde164...` on pinned base `74dd94d3` | review + README identity |
| Author-arranged Cursor review | no blocking source findings on complete candidate `17fde164...` | `review/` |
| Latest `main` vs proof head | `09df29cc` one unrelated test file; merge-tree clean; no feature change | `review/current-main-merge-check.json` |
| Native acquire of live lease | exit 0; lease `cbx_406764ae8f7a`; VM `f3f156aa-...` | `readiness/acquire-ssh.txt` |
| Exact claim before extra config | `sshPort=22`, host `local`, claimed `2026-09-17T11:43:50Z` | `reuse/exact-claim-before-configuration.json` |
| Isolated generic SSH port | config shows `:2222`; no ssh-port override | `readiness/config-port-default.txt` |
| Saved-port reuse | `sw_vers` `26.5.2` / `25F84`, exit 0 | `reuse/reuse-saved-port.txt` |
| Combined reuse trace | default 2222, claim 22, empty Tools IP, 22 open / 2222 closed, fresh CLI no override, `sw_vers` exit 0 | `reuse/reuse-command-trace.txt` |
| Independent ports after Tools disable | 22 open / 2222 closed; Tools IP list empty | `reuse/independent-port-observation.json`, `reuse/clone-no-tools.json` |
| Operator clone-only Node/npm | Node `22.23.1` / npm `10.9.8`, exit 0 | `readiness/clone-node-prerequisite.txt` |
| Source initial full disk hash | both HDS; shasum and OpenSSL agree | `source/source-disk-before.sha256`, `source/source-disk-openssl-before.txt` |
| After-reuse + post-cleanup config/unrelated digests | all 7 match original source baseline | `source/source-config-after-reuse.sha256`, `source/post-cleanup-audit.json` |
| Post-cleanup source disk hash compare | both HDS AFTER match BEFORE; sizes/mtimes match; `result=PASS` | `source/source-disk-after.txt`, `source/source-final-comparison.json` |
| Exploratory preflight clone | UUID `15829ace-...` deleted earlier | `source/preflight-cleanup-confirmed.txt` |
| Guarded live-lease cleanup | first shutdown refused delete; retry exact-UUID forced stop + native Crabbox `stop` exit 0 at `12:11:50Z`; bundles/claims/keys/metadata absent | `source/guarded-final-cleanup.txt`, `source/guarded-final-cleanup-retry.txt`, `source/post-cleanup-audit.json` |

## Open / failed / pending

| Item | State | Do not claim |
| --- | --- | --- |
| Account-auth RFB + visible nonce | **FAILED** before RFB (OD `-2700`, OD `5100`, then System Policy deny on kickstart). Normal clone-local System Settings or MDM setup is needed before another desktop proof. | desktop success |
| Current-head CI | **action_required** on 4 workflow runs; `pull` only | CI green / maintainer approval |
| Maintainer assessment | **NOT GRANTED** | merge readiness |
| Official Autoreview | **NOT RUN** | official review |

## Disqualified

The 2026-09-16 source-changing proof is **DISQUALIFIED**. Other-host source `b98e38c2-...` was historically modified and was not operated this task. Do not treat that packet’s desktop follow-up, source-side repairs, or any later reuse of that source VM as this run’s acceptance.

## Parent-stated, not independently re-queried here

- PR 1745 `headRefOid=03de7896...` is `MERGEABLE` and `mergeStateStatus=BLOCKED` (`review/final-pr-state.json`). This packet did not read or write GitHub.
- Publisher permissions are `pull` only (`review/github-permissions.json`).
- Operator installed Node `22.23.1` / npm `10.9.8` on the live clone only. Versions are in `readiness/clone-node-prerequisite.txt`.
- First node-launched acquire `cbx_56d2cacc3552` / VM `48d5af14-...` was cancelled and cleaned. Its remote log is not published here. Post-cleanup audit records that lease material absent.
