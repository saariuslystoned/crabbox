# PR 1745 E2E publication packet — 2026-09-17

**Status: INCOMPLETE.** Sanitized evidence index for Crabbox PR 1745 live work on 2026-09-17. Not maintainer acceptance, not official Autoreview, and not desktop or CI closeout.

The 2026-09-16 source-changing packet is **DISQUALIFIED**. Other-host source `b98e38c2-2d06-403c-872f-6315bc68b461` was historically modified and was **not operated** this task.

## Revision binding

| Item | Value |
| --- | --- |
| Final PR SHA | `03de7896dd094e5feb7c74d480ce3995d2c8760f` |
| Review / proof diff base | `74dd94d304fae007b27dd0bb7268718023b0ace6` |
| Latest `main` (unrelated) | `09df29ccc90bef484257756b2a322cb698761918` |
| Candidate diff SHA-256 (`74dd...03de`) | `17fde16497172e0bf672b6ff858f8969e15bcd62384f5e82e0d0b6d4149a4971` |
| Final binary SHA-256 | `7db4eea83126f9f888d7a491e29be7eb01cfd506633c322e3da4840f01639cc6` |
| Branch / PR | `fix/parallels-macos-bootstrap` / [PR 1745](https://github.com/openclaw/crabbox/pull/1745) |
| PR state | `MERGEABLE`, `mergeStateStatus=BLOCKED` |
| Current-head CI | 4 workflow runs `action_required`; publisher `pull` only |
| Live lease / clone | gone (`cbx_406764ae8f7a` / `f3f156aa-93c3-4656-848c-05d472a57916`) |
| Source UUID | `d1cc3c01-c6a0-43a1-82c0-61b841f8f636` stopped, unused as a write target |
| Unrelated UUID | `58b49202-a6de-4e24-ba8f-c1890c778535` suspended, unused |
| Deleted preflight clone | `15829ace-273d-428d-8a4b-70042b67450b` |
| Exact claim written | `2026-09-17T11:43:50Z` |
| Saved-port reuse | `/usr/bin/sw_vers` → `26.5.2` / `25F84`, exit 0 |
| Guarded cleanup | retry PASS `2026-09-17T12:11:50Z` |

## What this packet is allowed to claim

Limits are in [STATUS.md](STATUS.md).

- Author-arranged Cursor implementation reached clean SHA `03de7896` on pinned base `74dd94d3`. Latest `main` `09df29cc` is one unrelated Linux installer test file; no feature change. Review hash stays on `74dd...03de`.
- Author-arranged Cursor review of the complete candidate hashed `17fde164...`; that hash matches committed `74dd...03de`.
- Native full-clone acquire of the live lease succeeded (`acquire-ssh.txt` exit 0).
- Exact local claim existed before extra clone configuration (`sshPort=22`, `labels.host=local`).
- Reuse with generic config port 2222 and no `--ssh-port` used saved port 22. Combined trace: default 2222, claim 22, Tools IP empty, 22 open / 2222 closed, fresh CLI `sw_vers` exit 0.
- Operator clone-only Node `22.23.1` / npm `10.9.8`.
- Source initial disk hashes are complete for both HDS (shasum + OpenSSL agree). After-reuse and post-cleanup 7 config digests match the original source baseline. Post-cleanup AFTER OpenSSL hashes (`12:12:10Z`–`12:15:16Z`) match both BEFORE values; comparison `result=PASS`.
- Exploratory preflight clone was deleted earlier. Final live lease/clone later released; inventory then showed only the stopped source and suspended unrelated VM. Bundles, claims, keys, and metadata are absent.
- Native account `cbxdesk1745c` UID 504 was created and password-verified via prompted `sysadminctl`. OS System Policy denied kickstart `RemoteManagement.launchd`. No RM permissions, no RFB, no nonce. This run stopped; completing desktop proof requires normal clone-local System Settings or MDM setup.
- First guarded cleanup refused delete after guest shutdown (inventory never observed stopped). Retry used exact-UUID forced stop, independently rechecked stopped, then native Crabbox `stop` of the exact claim succeeded.

## Open gates

1. **Account-authenticated RFB / visible desktop effect** — not proven. Stopped at System Policy on kickstart. Normal clone-local System Settings or MDM setup is needed before another desktop proof.
2. **Current-head maintainer CI** — 4 runs `action_required`. No upstream push/admin rights.
3. **Maintainer independent assessment / merge** — not granted by author-arranged Cursor review and not granted by this packet.

Official Autoreview was not invoked. Full-suite host tests are **not green**.

## Evidence index

| Folder | Role |
| --- | --- |
| [STATUS.md](STATUS.md) | Incomplete-status declaration |
| [source/NOTES.md](source/NOTES.md) | Source/unrelated inventory; before/after hashes; guarded cleanup |
| [readiness/NOTES.md](readiness/NOTES.md) | Native bootstrap vs operator Node/npm |
| [reuse/NOTES.md](reuse/NOTES.md) | Saved-port reuse and Tools/IP caveats |
| [desktop/NOTES.md](desktop/NOTES.md) | Failure before RFB; System Policy blocker |
| [review/NOTES.md](review/NOTES.md) | Cursor review ≠ maintainer acceptance; CI/PR state |
| [test/author-arranged-local-verification.md](test/author-arranged-local-verification.md) | Author-arranged host tests; full suite not green |
| [MANIFEST.sha256](MANIFEST.sha256) | SHA-256 of every published file except itself |

## Sanitization

| Token | Replaces |
| --- | --- |
| `$TASK_ROOT` | absolute task/work-root prefix |
| `$SOURCE_ROOT` | source VM bundle prefix |
| `$HOST_HOME` | Parallels-host home prefix outside `$TASK_ROOT` / `$SOURCE_ROOT` |
| `$CHECKOUT` | implementation worktree path |
| `$PROVIDER_KEY` | non-public provider-key label |
| `unrelated-vm` | unrelated VM display name / bundle name |

Kept: lease IDs, VM UUIDs, SHAs/digests, times, statuses, guest IPs/MACs, guest account `parallels-02`, Crabbox run IDs, `sw_vers` output.

Excluded: internal orchestration, agent/session IDs, helper source, credentials, SSH key bodies, signed tracker query strings, the 2026-09-16 disqualified packet, and any claim of desktop success or maintainer acceptance.

## Distinctions

[review/cursor-independent-review.md](review/cursor-independent-review.md) is an **author-arranged Cursor source review**. It is not maintainer acceptance, not official Autoreview, and not live-VM proof. The implementation report is the same class of author-arranged closeout.

Crabbox native acquire/bootstrap in this run is guest SSH readiness on the **clone** (lease key / ready-check). It is not an automatic Node/npm install. Because the source image lacked Node, the operator installed official Node `22.23.1` / npm `10.9.8` **clone-locally** after native SSH preparation. See [readiness/NOTES.md](readiness/NOTES.md).

This packet records the completed run; acceptance remains incomplete for the open gates above.
