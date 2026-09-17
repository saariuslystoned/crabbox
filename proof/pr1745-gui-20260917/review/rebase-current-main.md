# PR 1745 rebase onto current main

Isolated worktree rebase only. No push, no VM, no credentials, no GitHub. Other worktree left alone.

## Result

Rebase succeeded. One changelog conflict; no source/provider-default/SSHFS conflict.

| Item | Value |
| --- | --- |
| Worktree | `$CHECKOUT` |
| Branch | `work/pr1745-gui-final-20260917` |
| Old HEAD | `1982f4e8fe07b5bf9e43778ab26fe18bad637299` |
| Old base | `74dd94d304fae007b27dd0bb7268718023b0ace6` |
| New base | `f288eb7a13142fb05cbcad3c65c12e966df77044` (`upstream/main`, `refactor(cloudflare): share instance type resolution policy (#2311)`) |
| New HEAD | `fab0aee2bee2dd28bded5444bf341f3bd3d61212` |
| Range | `f288eb7a..fab0aee2` (5 commits) |
| Other worktree | `$CHECKOUT_E2E` still `1982f4e8` on `work/pr1745-e2e-20260917` (not reset/edited) |

## Range-diff (`74dd94d3..1982f4e8` vs `f288eb7a..fab0aee2`)

```
1:  566ff498 = 1:  ace2e552 fix(parallels): bootstrap macOS guests without Tools
2:  3b17d585 = 2:  71efce04 feat(parallels): support macOS account desktop auth
3:  0b6fb2bf = 3:  2e6a56b3 fix(parallels): harden macOS lease reuse
4:  03de7896 = 4:  b02827d9 test(parallels): cover exact-claim SSH port with discovery errors
5:  1982f4e8 ! 5:  fab0aee2 fix(parallels): allow owned macOS clone desktop without a label
```

Commits 1–4 identical. Commit 5 differs only in `CHANGELOG.md` context: kept the 1982 manual-desktop gate bullet and the upstream PR 1556 native-filesystem / `cp --recover` bullets.

## Conflict / preservation

- Conflicted file: `CHANGELOG.md` only (last pick).
- Upstream provider-default refactor and native filesystem/SSHFS runner applied cleanly (`ApplyLinuxConnectionDefaults` exported; inlined DigitalOcean/Vultr/Linode/Lambda/Nebius/OVH/Scaleway/TencentCloud defaults remain in adapters; `internal/cli/remote_filesystem*.go` present).
- Preserved from 1982: trusted `parallels.bootstrapKey` / `parallels.password` (repo config cannot supply them), claim/port/error/ownership hardening, RFB password child-env scrub, `parallelsOwnedMacOSDesktopLease` manual desktop gate (source/unowned still rejected).

## Test / build (Go 1.26.5, no VM)

| Check | Result |
| --- | --- |
| `go vet ./internal/providers/parallels ./internal/cli` | pass |
| `go test -race -timeout=20m -count=1 ./internal/providers/parallels` | pass (`ok` 1.700s) |
| `go test -race -timeout=20m -count=1 ./internal/cli -run 'TestParallels\|TestApplyParallels\|TestApplyFileParallels\|TestApplyProviderConfigDefaultsReturnsMissingParallels\|TestParallelsBootstrapKey\|TestParallelsPassword\|TestEnforceManagedLeaseCapabilities\|TestValidateCoordinatorLeaseCapabilities\|TestExternalDesktopChildEnv\|TestApplyTargetChildEnvironment\|TestControllerChildEnvironment\|TestWebVNCDaemonChildEnvironment\|TestDesktopCredentials'` | pass (`ok` 3.144s) |
| `go build -trimpath -o bin/crabbox ./cmd/crabbox` | pass; SHA256 `de4a7e70f8e8b91351190209b25961ad9c4e582985a0dc1f41bddccf7124f08e` |

Includes the 1982 gate tests `TestEnforceManagedLeaseCapabilitiesAllowsOwnedParallelsMacOSManualDesktop` and `TestEnforceManagedLeaseCapabilitiesRejectsParallelsSourceAndUnownedDesktop`. Native RFB typing fix not on this branch (other agent WIP; not fetched).

## Not done

No push. Parent can cherry-pick later.
