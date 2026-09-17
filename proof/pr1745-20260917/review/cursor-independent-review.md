**No blocking findings.** Source review of the rebased PR plus the current working-tree test delta does not show a broken ownership, discovery, claim-port, password, or macOS desktop-auth boundary.

## Reviewed identity

| Item | Value |
| --- | --- |
| Worktree | `$CHECKOUT` |
| Branch | `work/pr1745-e2e-20260917` |
| HEAD | `0b6fb2bf7dad951a8273286489a8df3c263281bd` (`fix(parallels): harden macOS lease reuse`) |
| Base `upstream/main` | `74dd94d304fae007b27dd0bb7268718023b0ace6` |
| Stack | `566ff498`, `3b17d585`, `0b6fb2bf` |
| Committed `upstream/main...HEAD` SHA-256 | `ed89dc2a0ed7603838df4d2c6e9fb23ad527d406d93b95c35f9d2c49fa21639d` |
| Uncommitted `HEAD` working SHA-256 | `5df077876b624a5bf99ff7da81bf52afcdb83c634435e6611eaf36b0fdbddc9e` |
| Complete candidate (`upstream/main` including dirty) SHA-256 | `17fde16497172e0bf672b6ff858f8969e15bcd62384f5e82e0d0b6d4149a4971` |

Dirty state is test-only: unstaged `internal/providers/parallels/provider_test.go` (exact-claim port preservation after failed discovery, plus `ip_source` / SSH host assertions).

## Implementation acceptance (source)

**Ownership / source immutability.** Release and cleanup still require an exact local claim bound to lease ID, VM ID, and host (`backend.go` `exactParallelsClaimOwned`, `requireExactParallelsClaim`). Status may inspect without adopt; reuse without `--reclaim` is refused. Linked clone still requires an explicit snapshot so `prlctl` cannot create a source-side linked-clone snapshot (`parallels.go` `Clone`). Bootstrap SSH targets the clone IP from Tools or exact-MAC DHCP, not the source name/ID. Guest prep never sends `dscl`/`-passwd`.

**DHCP/MAC and host SSH.** Fallback is macOS-plus-absolute-`bootstrapKey` only. Matching is exact normalized MAC, fail-closed on missing/stale/malformed/ambiguous leases, then host-side TCP probe of the configured/saved SSH port (`WaitForIP`, `resolveParallelsDHCPLeaseIP`). Remote cat/nc/ssh stay on the Parallels host; host key is `-i` + `IdentitiesOnly` when set. Guest bootstrap uses that host-side key, `BatchMode`, no password/kbd-interactive, no agent/X11 forward.

**Exact-claim port before discovery.** On exact claim, `applyParallelsClaimSSHPortToConfig` rewrites `SSHPort` and clears fallbacks before `WaitForIP`; the same fence is applied to the returned SSH target after discovery (`backend.go` 202–208, 241–243). Mismatched lease/VM/host does not adopt a saved port.

**Upstream #2301.** Connection resolve still returns the original discovery error (`errors.Is(..., context.Canceled)` in tests). Status/release swallow discovery failure and keep the matched VM identity; the dirty test also keeps saved port `22` and empty SSH host on that path.

**Password trust and child scrubbing.** `parallels.password` applies only when the file is trusted; env overlay is `CRABBOX_PARALLELS_PASSWORD`; there is no password flag (`config.go` 4011–4014, 6122; `provider.go` 174–176). Templates/hosts cannot carry it. Config show emits only `configured`/`missing`. The value is used as a boolean for guest prep, never interpolated. `NewParallelsClient` and `parallelsChildCommandEnv` strip the env name; `parallelsSSHTarget` denylists it for ssh/scp/rsync/ProxyCommand/viewer children.

**macOS account desktop contract.** `DesktopCredentials` is macOS-only, requires a configured password, uses the lease SSH user (else `parallels.user`), and selects ARD in `resolveMacOSWebVNCCredentials`. With a password, guest prep skips generated VNC password / `setvncpw` and only enables Screen Sharing for that user.

Bounded credential-free unit tests for those packages/names passed with Go `1.26.5` against this dirty tree. That is still not live VM evidence.

## Limitations (not live proof)

- No VM, host, DHCP file, SSH, ARD, or Screen Sharing execution.
- Official Autoreview was not invoked: this pass was read-only and the tree contains synthetic credential-shaped fixtures that must not be forwarded.
- Full `-race ./...` was not rerun; the owner is already running it.
- Working-tree SHA is the complete candidate including uncommitted tests; HEAD alone is the three rebased commits.