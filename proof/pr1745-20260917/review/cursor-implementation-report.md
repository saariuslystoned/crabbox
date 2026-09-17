Rebase and verification are complete. Branch `work/pr1745-e2e-20260917` is clean, not pushed, and sits on current `upstream/main`.

## Result

| Item | Value |
|---|---|
| **Final SHA** | `03de7896dd094e5feb7c74d480ce3995d2c8760f` |
| **Upstream base** | `74dd94d304fae007b27dd0bb7268718023b0ace6` (`fix(parallels): preserve IP discovery results`, PR #2301) |
| **Binary** | `bin/crabbox` SHA256 `7db4eea83126f9f888d7a491e29be7eb01cfd506633c322e3da4840f01639cc6` (`build_exit=0`) |

History on top of main:

1. `566ff498` `fix(parallels): bootstrap macOS guests without Tools`
2. `3b17d585` `feat(parallels): support macOS account desktop auth`
3. `0b6fb2bf` `fix(parallels): harden macOS lease reuse`
4. `03de7896` `test(parallels): cover exact-claim SSH port with discovery errors`

## Diff vs `upstream/main`

10 files, **+1444 / −86**. Scope stays Parallels-only: `docs/providers/parallels.md`, CLI config/parallels/help-contract tests, and `internal/providers/parallels/*`.

Conflicts were only in `provider_test.go` (commit 1) and `backend.go` (commit 3). Resolve now does this, in order:

1. Restore the exact-claim SSH port onto the client config **before** `WaitForIP`
2. Recreate the client so DHCP/TCP probe uses that port
3. On discovery error: **connection** returns the original error; **status/release** stay best-effort and keep VM identity
4. Assign `server.PublicNet.IPv4.IP` once after discovery so SSH host and server address match
5. Record `ip_source` when DHCP/MAC fallback wins

Password stays config/`CRABBOX_PARALLELS_PASSWORD` only. It is denylisted from child env and never appears as a CLI flag.

## Help contract

Inspected actual `run --help` from binaries built from this tree and a pristine `upstream/main` archive.

| Tree | SHA256 | Bytes |
|---|---|---|
| main | `18091638a89f84fd3295b05b23579f46acedf9f0ebae42fd587e62fed6e08773` | 62201 |
| this branch | `a69acd0a8b8062df43dc77899ee82bc9f31e848a3617660d9cd84400f09bec94` | 62323 |

Delta is only `--parallels-bootstrap-key`. Existing contract left unchanged.

## Checks

| Check | Exit |
|---|---|
| `go test -race -timeout=10m ./internal/providers/parallels/` | **0** |
| focused `./internal/cli/` Parallels + help-contract filter | **0** |
| `go vet ./...` | **0** (`vet_exit=0`) |
| `go test -race -timeout=20m ./...` | **1** (`full_suite_exit=1`) |
| `go build -trimpath -o bin/crabbox ./cmd/crabbox` | **0** |

Feature-local packages passed in the full run (`internal/providers/parallels` 6.713s). `cmd/crabbox` also passed (62.026s), which includes the built-binary help contract.

## Full-suite failures (not masked)

Host `umask=077`. The same CLI and apple-machine failures reproduce on a pristine `upstream/main` archive (`main_cli_exit=1`, `main_applemachine_exit=1`).

**Reproduce on main (file-mode / umask / toolcache):**

- `TestGitHubActionsRunnerSeedsOnlyOwnedDefaultToolCache` — `unavailable or unauthenticated image slot`
- `TestPublishedArtifactsSummaryModeFollowsURLKind` — mode `0600` vs unrestricted
- `TestPublicAndLocalArtifactManifestPreserveSharedModes` — manifest `0600`, want `0644`
- `TestNativeImagePreparationPreservesCurrentBootStatus` — completion file `-rw-------`
- `TestControllerStateLockRejectsSymlinkWithoutChangingTarget` / `...BroadExistingFileWithoutChmod` / `TestReadControllerTokenRequiresPrivateRegularFile`
- `TestCopyOverResolvedSSHArchiveFallbackWithOldRsync`, `TestRecoverCopyArchiveTransaction/reject_forged_marker`
- `TestGitOverlayRsyncFilesFromDoesNotApplySourceRootMetadata` — dest `0700`, want `0751`

**Reproduce on main (apple-machine host layout):**

- `TestRunUsesHomeMountedRepoAndEnv`, `TestRunTimingJSONClassifiesCommandFailure`, `TestRunDeletesOneShotMachineSession` — home mount cannot exclude managed state namespace
- `TestAcquisitionRollbackRequiresOriginalBindingAndClaim` — claim/cleanup assertions

**Load/timeout only (pass isolated on both trees):**

- `internal/cli` hit `panic: test timed out after 20m0s` while `TestWorkspaceOwnerPOSIXSetupSignalDenialClosesStreams` was running
- `TestWorkspaceOwnerDashSignalHandoff` failed in the full run (`dash` exit 127); isolated **0** on this tree and main
- `TestBlacksmithDownloadInstalledHelperExecution` / `TestBlacksmithDownloadCancellationRetainsClaimUntilClosure` failed under suite load (`signal: killed`, deadline); isolated **0** on this tree (`worktree_blacksmith_isolated_exit=0`) and main (`main_blacksmith_exit=0`)

## Remaining blockers

1. **Not pushed** — GitHub was not mutated, as required.
2. **Full-suite host gates** — `umask=077` plus this machine’s toolcache/apple-machine state layout. They fail on pristine main too; do not treat them as PR #1745 regressions.
3. **CLI 20m package timeout** under `-race ./...` on this host. Isolated Parallels/help/dash checks pass.
4. **Review/merge** still needs a human or a later push. No CHANGELOG was added in these contributor commits.