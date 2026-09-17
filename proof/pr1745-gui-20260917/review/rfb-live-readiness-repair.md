# PR1745 RFB live input-readiness repair

Status: **source repair only. Live native type is not claimed.**

Parent live regression on `ff5d4cd55417dd81ddef0f859164f46c43b5b1ad` binary `697bbac5` remains the last observed desktop result: `desktop type` of `PR1745_NATIVE_FINAL_FF5D4CD5_CC38` exited 0 with a blank TextEdit, and the second fresh invocation `_NATIVE_FF5D4CD5_SECOND` also produced no characters. Do not treat this commit as a successful nonce.

## Compare

Native `typeRFBTextFromConn` on `ff5d4cd5` authenticated ARD, sent ClientInit `shared=1`, waited for one framebuffer, typed immediately, then drained one framebuffer. That unread-framebuffer wait was necessary and is preserved. It was not sufficient.

The same-session helper (not modified) at `proof/crabbox-pr1745-e2e-20260917/live-helper/rfbprobe/session.go` used the same auth + first-frame path, then spent ~2s before the nonce that actually landed: save PNG, Command-Space, wait 500ms, type `TextEdit` (often absent; Spotlight often did not open), wait 1200ms, type nonce, wait 80ms, read frame. First keys were dropped; later keys in the same session appeared. That matches asynchronous Apple Screen Sharing control acquisition after session init, not a missing first-frame read.

## Handshake vs delay

RFB 3.8 and Apple ARD type 30 have no input-ready server message after ServerInit. Tight/QEMU fence and continuous-update encodings are not advertised by Screen Sharing, so there is no supported readiness handshake to poll. The repair is a documented 2s cancel-aware settle after auth and the first framebuffer, before the first real key. Auth failure, missing ready frame, and missing drain frame still fail as before. The settle, the drain frame, and the CLI byte count are not glyph proof.

## This binary

- Worktree: `.worktrees/crabbox-pr1745-gui-final-20260917`
- Branch: `work/pr1745-gui-final-20260917`
- Commit: `0095e1a32cc6379ff0d401cd5cbd4adb5fe20923`
- Binary: `bin/crabbox` (`go build -trimpath -o bin/crabbox ./cmd/crabbox`)
- Binary SHA256: `27c722172163ac237a29a687957378a069f0ce3ab411c8e119920b48d36063c9`
- Helper: unchanged

## Verification run here

- `gofmt` on the two RFB files
- `go vet ./internal/cli/`
- `go test -count=1 -timeout=60s ./internal/cli/ -run 'TestTypeRFBText|TestRFBInputReadySettle|TestRFBTextKeyEvents|TestRFBKeysym|TestCaptureRFB|TestPreflightRFB|TestNegotiateRFB'`
- same filter with `-race`
- no VM SSH, no GitHub credentials, no live native type, no glyph claim

New tests cover: settle occurs after the ready framebuffer and before the first KeyEvent; cancel during settle returns `wait for RFB input ready: context canceled` without sending keys and without waiting the full interval. Existing auth / ready-frame / drain-frame tests kept.

Parent still holds `cbxfinal1745` and can swap this binary without a new account. Live result is pending parent retest on a blank TextEdit document.
