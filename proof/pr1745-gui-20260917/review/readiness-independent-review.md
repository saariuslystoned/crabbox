# Independent review — PR 1745 RFB input-ready settle `0095e1a3`

Read-only source review of `ff5d4cd5..HEAD` after the implementer committed the narrow settle repair. No VM, SSH, GitHub, secrets, or repo edits. Tests not rerun (implementer ran). Parent owns any live native result. This file does not claim maintainer acceptance or live pass.

Prior reviewed surface is `final-integration-review.md` on `ff5d4cd5`. This pass checks only the new settle delta plus that those invariants still hold.

## Verdict

**No blocking findings.**

## Identity

| Item | Value |
| --- | --- |
| HEAD | `0095e1a32cc6379ff0d401cd5cbd4adb5fe20923` |
| Parent | `ff5d4cd55417dd81ddef0f859164f46c43b5b1ad` |
| Tree | `bdbd46c97630a5431a1b8e64c966d6b69f8bcfc7` |
| Branch | `work/pr1745-gui-final-20260917` |
| Worktree | `$CHECKOUT` |
| Porcelain | empty |
| Diff hash `ff5d4cd5..HEAD` | `9b19e5b950d556748f60ebb82a823107cdd436a4` |
| Files in newdiff | `CHANGELOG.md`, `internal/cli/rfb_screenshot.go`, `internal/cli/rfb_screenshot_test.go` |
| Clone-gate files vs `fab0aee2` | identical (`capabilities.go`, `docs/features/capabilities.md`, `coordinator_capabilities_test.go`) |
| Implementer repair note | `proof/crabbox-pr1745-gui-20260917/rfb-live-readiness-repair.md` (source repair only; live not claimed there) |
| Observed `bin/crabbox` SHA-256 | `27c722172163ac237a29a687957378a069f0ce3ab411c8e119920b48d36063c9` (file present; not live-exercised here) |

Diff hash is `git --no-replace-objects diff --no-ext-diff --no-textconv ff5d4cd5 HEAD | git hash-object --stdin`.

## Blocking findings

None.

## Newdiff (independent)

Order on the type path is now:

1. Credential auth + ClientInit + ServerInit (`requireAuth=true`)
2. SetPixelFormat + SetEncodings
3. FramebufferUpdateRequest + read (`wait for RFB session ready`)
4. Cancel-aware `waitRFBInputReady` (`defaultRFBInputReadySettle = 2s`)
5. KeyEvent down/up with existing 5ms pacing
6. FramebufferUpdateRequest + read (`drain RFB session after typing`)

Ready/drain images remain discarded (`_, err :=`). Success is still protocol consume, not OCR or glyph matching.

The 2s interval is a package default with an unexported test hook. Comments, changelog, and `TestRFBInputReadySettleIsDocumentedDelay` all state it is a documented delay, not an RFB/ARD handshake, and not glyph proof. That matches the stated parent failure (native `ff5d4cd5` exit 0 / 33 bytes / blank TextEdit) versus the helper’s ~2s post-frame wall clock before a full nonce. There is no supported input-ready server message after ServerInit; Tight/QEMU fence and continuous-update encodings are not advertised. Skipping the settle cannot false-pass the new first-key timing test (`>= 40ms` after the ready frame). Existing success servers still reject a KeyEvent before pixel format / encodings / the ready request.

`waitRFBDuration` is the shared cancel-aware wait. `d <= 0` returns immediately so retained tests can pin settle to 0 without sleeping 2s.

## Retained invariants from `ff5d4cd5`

- **Owned-clone / manual desktop gate:** not in the newdiff; files identical to `fab0aee2`.
- **Auth None rejected on type:** `initializeRFBConnection(..., true)` still returns `RFB server did not require credential authentication` before ClientInit. `TestTypeRFBTextRequiresCredentialAuthentication` and preflight None reject are unchanged. Click/screenshot still pass `requireAuth=false`.
- **Auth failure:** `TestTypeRFBTextFailsOnAuthenticationFailure` unchanged.
- **Ready/drain required:** missing-ready and missing-drain tests kept; both pin settle to 0 so a 1s context timeout cannot expire in the new wait and mask those errors.
- **Context cancel:** `applyRFBConnDeadline` still returns `ctx.Err()` first (`TestTypeRFBTextHonorsCanceledContext` still cancels before the call). Key-loop and inter-key delays still watch `ctx.Done()`. New `TestTypeRFBTextHonorsCanceledContextDuringInputReady` cancels after the ready frame and requires `wait for RFB input ready` + `context.Canceled` without a KeyEvent and without waiting the full settle.
- **Glyph proof not claimed:** changelog: “The wait is not glyph proof.” Drain frame and CLI byte count are not treated as visible text.

## Non-blocking (do not reopen as blockers)

- The 2s value is helper-session wall clock, not a measured control-attach signal. Live native type remains parent-owned.
- Click still has no ready/drain/settle. Out of scope.
- `rfbInputReadySettle` is a package var. This test file has no `t.Parallel()`.
- Cancel-during-settle elapsed includes handshake time against a 250ms bound. Adequate for `net.Pipe` ARD, not a correctness hole.

## Live / acceptance

Not reviewed here. Do not treat this file as native glyph delivery, CI green, or maintainer sign-off.
