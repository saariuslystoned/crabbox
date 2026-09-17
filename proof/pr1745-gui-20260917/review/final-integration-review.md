# Final integration + independent review — PR 1745 GUI `ff5d4cd5`

One-shot Cursor review and integration. No VM, GitHub, credentials, or push. Old implementation worktree left untouched. Parent still owns live E2E with `cbxfinal1745`; this file does not claim native glyph delivery.

## Verdict

**No blocking findings.** Cherry-pick of `a418ac44` onto this isolated rebase is sound.

Type no longer accepts RFB None. Ready/drain wait for actual framebuffer reads before and after KeyEvents. The returned frame is discarded and is not treated as proof that glyphs appeared. The owned-clone / manual desktop gate from `fab0aee2` is unchanged.

## Identity

| Item | Value |
| --- | --- |
| Final HEAD | `ff5d4cd55417dd81ddef0f859164f46c43b5b1ad` |
| Tree | `32a4f44c5e77bda3c3619d75853e355a3aecbfe2` |
| Branch | `work/pr1745-gui-final-20260917` |
| Worktree | `$CHECKOUT` |
| Base (current main, rebase target) | `f288eb7a13142fb05cbcad3c65c12e966df77044` |
| Pre-integration HEAD | `fab0aee2bee2dd28bded5444bf341f3bd3d61212` |
| Cherry-picked commit | `a418ac442df93a62d6cf9d9b8ddf20e893fe78d4` |
| a418 parent | `1982f4e8fe07b5bf9e43778ab26fe18bad637299` |
| `f288eb7a` is ancestor of HEAD | yes |
| `fab0aee2` is parent of HEAD | yes |
| Diff hash `f288eb7a..HEAD` | `3c0ace1ed6ab40ee2134e183899edf938809896e` |
| Diff hash `fab0aee2..HEAD` | `932ee385e32df4b0471c9295ea0e4297cdcf96e7` |
| Diff hash `1982f4e8..a418ac44` | `f6f38384a0db0309414371d5e13f87f973add701` |
| RFB sources vs a418 | identical |
| Clone-gate files vs `fab0aee2` | identical (`capabilities.go`, `docs/features/capabilities.md`, `coordinator_capabilities_test.go`) |
| Old implementation worktree | `$CHECKOUT_E2E` still `a418ac44`, porcelain empty |

Diff hashes are `git --no-replace-objects diff --no-ext-diff --no-textconv <a> <b> \| git hash-object --stdin`.

## Finished binary for live E2E

Parent should pin this exact file:

| Item | Value |
| --- | --- |
| Path | `$CHECKOUT/bin/crabbox` |
| SHA-256 | `697bbac596c5995ec7b993a7e296657097d67b339f98e29a4e086a2903ec56e3` |
| Size | 149799026 bytes |
| File | Mach-O 64-bit executable arm64 |
| Build | `go1.26.5` `go build -trimpath -o bin/crabbox ./cmd/crabbox` from HEAD `ff5d4cd5` |
| `crabbox version` | `dev` (local untagged trimpath build) |

Do not claim native `desktop type` pass until parent live-verifies with `cbxfinal1745`. The 1982 native run reported 22 bytes and only the first character was visible.

## Blocking findings

None.

## Independent review of `a418ac44` against `1982f4e8`

Reviewed the raw parent/commit objects and `git --no-replace-objects diff --no-ext-diff --no-textconv 1982f4e8 a418ac44` before cherry-pick. RFB sources on this rebase matched 1982 exactly, so the patch applied without touching the clone gate.

### Bug that must stay in scope

Native `desktop type` of `_CRABBOX_1982F4E8_1736` exited 0 and reported 22 bytes, but the guest document only gained the first `_`. The same-session helper waited for an initial framebuffer, typed, waited again, and the entire nonce was visible. Apple Screen Sharing can emit the first framebuffer immediately after ServerInit; if that write is unread, a single-threaded server blocks and never consumes later KeyEvents. Closing the SSH tunnel after a 50ms sleep then drops the unread events.

### Auth

- `typeRFBTextFromConn` calls `initializeRFBConnection(..., true)`.
- None (`rfbSecurityNone`) returns `RFB server did not require credential authentication` and does not send ClientInit.
- `TestTypeRFBTextRequiresCredentialAuthentication` offers only None, requires that error, and fails if the client continues with another byte.
- Failed ARD security-result is tested (`authentication failed`).
- Click and screenshot still pass `requireAuth=false`, so historical no-auth capture/click is unchanged. That is not a type-path accept-None hole.
- Preflight already rejected None and was not weakened.

### Readiness and drain

Order on the type path:

1. Credential auth + ClientInit + ServerInit
2. SetPixelFormat + SetEncodings
3. FramebufferUpdateRequest + `readRFBFramebufferUpdate` (`wait for RFB session ready`)
4. KeyEvent down/up for each rune, with cancel-aware 5ms pacing
5. FramebufferUpdateRequest + `readRFBFramebufferUpdate` (`drain RFB session after typing`)

The images from steps 3 and 5 are discarded (`_, err :=`). Success is protocol consume, not OCR or glyph matching. Changelog says Screen Sharing can consume the full text before the tunnel closes; it does not claim visible glyphs from the frame.

Missing-ready and missing-drain tests close the peer instead of writing a frame and require those exact error strings. Skipping either wait cannot false-pass those tests. The success server also rejects a KeyEvent before SetPixelFormat / encodings / the ready request.

### Context cancel and read failure

- `applyRFBConnDeadline` returns `ctx.Err()` before setting a deadline. `TestTypeRFBTextHonorsCanceledContext` cancels first; if that check were removed the `net.Pipe` would block and the test would hang or fail, not pass.
- Ready/drain read failures are tested with a 1s timeout and a server that closes after the matching request.
- Key-loop and inter-key delays also watch `ctx.Done()`.

### Non-blocking notes

Not blockers; recorded so later reviewers do not reopen scope:

- The 2×1 `net.Pipe` success frame cannot fill a TCP window the way a real Screen Sharing framebuffer can. The missing-frame tests still force the reads.
- None-reject happens after the client writes the selected security type and before reading an RFB 3.8 SecurityResult. The connection is aborted; that is acceptable.
- Click still does not ready/drain. Out of scope.

## Integration

`git cherry-pick --no-commit a418ac44` auto-merged. Only needed files changed:

- `internal/cli/rfb_screenshot.go`
- `internal/cli/rfb_screenshot_test.go`
- `CHANGELOG.md` (one RFB wait/drain bullet under Unreleased)

Preserved on purpose:

- Owned-clone desktop exception and fail-closed exact-claim bind (`parallelsOwnedMacOSDesktopLease`)
- Upstream Unreleased bullets for PR 1556 after the PR 1745 notes
- Old e2e worktree at `a418ac44`

Committed as `ff5d4cd5` with the a418 subject/body. Not pushed.

## Tests

Toolchain: `go1.26.5 darwin/arm64` at `go1.26.5` (mise shim had no version; did not change global mise config). Full suite not rerun.

| Command | Result |
| --- | --- |
| `go test -race -timeout=20m ./internal/cli -count=1 -run 'RFB\|Capabilit\|Parallels\|ChildEnv'` | `ok` 4.881s |
| `go test -race -timeout=20m ./internal/providers/parallels -count=1` | `ok` 1.720s |
| `go vet ./...` | exit 0 |
| `go build -trimpath -o bin/crabbox ./cmd/crabbox` | exit 0; SHA-256 `697bbac5…3ec56e3` |

Scoped CLI `-run` included the new type tests (`TestTypeRFBTextSendsExactKeyEventsAfterReadyAndDrain`, unicode, credential-auth required, auth failure, canceled context, missing ready, missing drain), other RFB capture/click/preflight tests, managed-lease capability / owned-clone / source-unowned cases, Parallels CLI + Parallels config/host/password tests, and child-environment scrub tests. Provider package was the complete parallels suite, including config-show and child-environment cases.

## Live E2E remaining (parent)

1. Pin the binary path and SHA-256 above.
2. Use new account `cbxfinal1745` only; do not reset or recover older proof accounts.
3. Require independent guest/prlctl visibility of the full typed nonce, not CLI byte count and not the drain framebuffer alone.
4. Keep source VMs untouched.
