# Cursor gate review — PR 1745 local commit `1982f4e8`

Independent read-only review. No repo edits, no host/GitHub operations, no tests/build (owned by the implementation agent).

## Verdict

**No blocking findings.** The new macOS Screen Sharing exception is fail-closed, clone-shaped, and bound to an exact local Parallels claim. It does not trust a fabricated `desktop=true` label, does not treat source/unowned VMs as desktop-capable, and does not skip RFB or SSH authentication.

## Identity

| Item | Value |
| --- | --- |
| Reviewed commit | `1982f4e8fe07b5bf9e43778ab26fe18bad637299` |
| Relative parent | `03de7896dd094e5feb7c74d480ce3995d2c8760f` |
| Parent is ancestor | yes |
| Subject | `fix(parallels): allow owned macOS clone desktop without a label` |
| Raw diff hash (`git diff --no-ext-diff --no-textconv 03de7896 1982f4e8 \| git hash-object --stdin`) | `daec0c4fadb05129829869e875735b3247918799` |
| Worktree | clean at HEAD `1982f4e8` on `work/pr1745-e2e-20260917` |
| Changed files | `CHANGELOG.md`, `docs/features/capabilities.md`, `internal/cli/capabilities.go`, `internal/cli/coordinator_capabilities_test.go` |

## Blocking findings

None.

## What the commit changes

`03de7896` rejected native Parallels reuse when `desktop=true` was missing. That matches the recorded E2E failure: GUI enabled Restricted RM Observe+Control on the owned clone, then `crabbox-screenshot` died in `enforceManagedLeaseCapabilities` before any RFB/SSH desktop probe.

`1982f4e8` keeps that label check, then adds one extra allow path:

1. `macOSScreenSharingLease` still admits coordinator-backed or provider-less macOS leases (preexisting Screen Sharing rule).
2. Native Parallels (`CoordinatorNever`) is no longer a blanket deny. `parallelsOwnedMacOSDesktopLease` may admit **only** an exact-claim Crabbox clone whose live name, lease id, CloudID, and host bind together.

Browser/code labels, non-default `desktop_env`, and static early-return are unchanged.

## Ownership and label-trust assessment

`parallelsOwnedMacOSDesktopLease` (`internal/cli/capabilities.go:155-179`) requires all of:

- provider is Parallels (`server.Provider` then `cfg.Provider`)
- VM name parses as `crabbox-cbx-<leaseid>-…` **and** that id equals the resolve lease id
- if `labels.lease` is present, it equals that same id
- live `CloudID` and `labels.host` are non-empty
- `ResolveLeaseClaimForProviderWithExact` returns `ok && exact`
- claim `LeaseID`, `CloudID`, and `labels.host` equal the live values

Exact-claim protection is real, not slug/alias recovery:

```1420:1435:internal/cli/claim.go
func ResolveLeaseClaimForProviderWithExact(identifier, provider string) (leaseClaim, bool, bool, error) {
	// ...
	if exists {
		if exact.LeaseID == "" || canonicalClaimProvider(exact.Provider) != provider {
			return exact, false, true, nil
		}
		return exact, true, true, nil
	}
	claim, ok, err := ResolveLeaseClaimForProvider(identifier, provider)
	return claim, ok, false, err
}
```

The helper then requires `exact` (`capabilities.go:176`). A slug-only or inexact recovery cannot open the desktop gate.

Fabricated labels are not sufficient:

- `desktop=true` on the Server object still takes the old label path. This change does not write or require that label.
- `target=macos` alone is not enough; Parallels must also pass the name/claim bind.
- `lease=<owned id>` on a source-named VM fails `parallelsLeaseFromVMName` (`claimed-source-vm` case).
- Renaming a source VM to `crabbox-cbx-<owned-id>-…` still fails because live `CloudID` is the prlctl UUID and will not match the clone claim.
- Host/CloudID are compared to the **claim file**, not to another attacker-chosen label pair.

Claim I/O errors now fail closed (`capabilities.go:109-111`, `173-175`) instead of being treated as “not macOS Screen Sharing.”

## sourceResolve path

Reviewed `internal/providers/parallels/backend.go` `Resolve` (`168-257`) and `authorizeParallelsResolve` / `exactParallelsClaimOwned` (`310-326`, `464-473`).

- Source VMs do not parse as Crabbox clone names. Resolve can still match them by raw UUID/name, but then `leaseID` becomes the VM UUID and `authorizeParallelsResolve` demands an exact claim bound to that UUID/host. Normal desktop commands are not `StatusOnly`/`ReleaseOnly`, so an unowned source fails **before** the new capability exception.
- Clone-shaped names get labels from `ParallelsLabelsFromName` (name + stored sidecar). Resolve then **overwrites** `labels.host` from the selected host and uses the live `vm.ID` as `CloudID`. Command-flag `desktop=true` is **not** copied onto named clones.
- Unlabeled/non-clone VMs still hit preexisting `DirectLeaseLabels(cfg.Desktop)` synthesis. That can stamp `desktop=true` only when `labels.lease` is empty, and reuse still needs `--reclaim`. That path is outside this exception and is not a new source bypass.
- Exact-claim SSH port restore (`parallelsClaimSSHPort`, `276-308`) uses the same lease/CloudID/host bind as the new desktop helper. The exception does not widen SSH identity.

The new helper is stricter than Resolve ownership: Resolve can own a reclaimed UUID that is not clone-named; the desktop exception additionally requires a `crabbox-cbx-<leaseid>` name.

## Native RFB / SSH path

The exception only skips the missing-`desktop=true` refusal. Desktop commands still:

1. Resolve the lease (owned clone + stored SSH key).
2. Call `enforceManagedLeaseCapabilities`.
3. Probe loopback `127.0.0.1:5900` over SSH (`waitForLoopbackVNC` / `probeLoopbackVNC` / `nc -z` on macOS).
4. Authenticate RFB.

Screenshot (`screenshot.go:48-62`) and macOS WebVNC (`macos_webvnc.go:109-127`) keep that order. `captureRemoteMacVNCScreenshot` / `resolveMacOSRFBAuthentication` (`rfb_screenshot.go:43-137`) still require provider ARD credentials or a managed password read over SSH, then real RFB security (VNC type 2 or ARD type 30). `SecurityNone` remains rejected.

Parallels credentials (`internal/providers/parallels/provider.go:174-188`) come from trusted config / `CRABBOX_PARALLELS_PASSWORD`, not from lease labels. SSH children still deny `CRABBOX_PARALLELS_PASSWORD`. Manual Observe+Control therefore still needs the GUI account password at RFB time; this commit does not invent or bypass that secret.

`TouchDirectLeaseLabels` does not write `desktop=true`, so a later touch will not forge the label this change is meant to avoid.

## Static / other-provider / macOS semantics

| Surface | After `1982f4e8` | Evidence |
| --- | --- | --- |
| Static SSH | still skipped before the new helper | `capabilities.go:105-107` |
| Coordinator-backed macOS (empty provider / non-`CoordinatorNever`) | still allowed without `desktop=true` | `capabilities.go:139-144`; existing `TestEnforceManagedLeaseCapabilitiesAllowsMacOSScreenSharing` |
| Coordinator lease API echo | unchanged; still requires `lease.Desktop` | `validateCoordinatorLeaseCapabilities` `run.go:4118-4121` |
| Tart / other `CoordinatorNever` macOS | still requires `desktop=true` | existing tart test + new tart case |
| local-container / Parallels Linux | still requires `desktop=true` when target is not treated as macOS | new table tests |
| Native Parallels macOS clone, exact claim, no desktop label | **new allow** | `TestEnforceManagedLeaseCapabilitiesAllowsOwnedParallelsMacOSManualDesktop` |
| Source name, claimed-source name, unowned clone, CloudID mismatch | still rejected | `TestEnforceManagedLeaseCapabilitiesRejectsParallelsSourceAndUnownedDesktop` |
| Browser / code labels | unchanged | `capabilities.go:123-128` |
| Non-default `desktop_env` | still required after the exception | `capabilities.go:117-121` |

No broad provider behavior change is in the diff. Only `internal/cli/capabilities.go` plus docs/changelog/tests.

## Tests

Added coverage is the right shape for this gate:

- owned clone allow without `desktop=true`
- source VM, claimed-but-source-named VM, unowned clone, claim bound to another CloudID
- Parallels Linux, Tart macOS, local-container still require the label
- claim I/O isolated via `XDG_STATE_HOME` (`CrabboxStateDir` honors that env)

Gaps (non-blocking; logic is present and fail-closed):

- no explicit host-mismatch, missing-host, lease-label-mismatch, or `exact=false` cases
- no claim-read error propagation case
- `macOSLeaseTarget` still ORs `cfg.TargetOS` with `labels.target` (preexisting coordinator rule). A Linux clone plus `--target macos` could theoretically satisfy the Parallels helper; `run` applies resolved labels first (`applyResolvedServerConfig`), screenshot/vnc do not. Not a source-VM hole.
- no new RFB/SSH tests; those paths are unchanged and were not re-proved here

Tests/build were not executed in this review.

## Checks performed

- Confirmed parent/commit objects and ancestor relationship locally
- Hashed the raw `03de7896..1982f4e8` diff
- Read the full four-file diff and current `capabilities.go`
- Read `ResolveLeaseClaimForProviderWithExact`, claim persistence of `CloudID`/`Labels`, and `CrabboxStateDir`
- Read Parallels `Resolve`, `authorizeParallelsResolve`, `exactParallelsClaimOwned`, `parallelsLeaseFromVMName`, `parallelsLabelsFromName`, host naming
- Read screenshot, vnc, macOS WebVNC, RFB capture, `resolveVNCEndpoint`, `providerDesktopCredentials`
- Compared new helper bind to existing exact-claim SSH port bind
- Compared new tests to preexisting coordinator/static/tart cases
- Did not run `go test` / build, `gh`, or any host/lease/GUI operation

## Non-blocking notes

- Docs still say a lease cannot grow a capability later (`docs/features/capabilities.md:26`) while the next paragraph documents this clone carve-out. Wording tension only.
- Functional E2E still depends on SSH to the clone and RFB auth with the manually granted account. This commit only removes the pre-RFB label refusal for an already-owned clone.

## Gate recommendation

**Pass for this commit’s safety/ownership contract.** Remaining work is implementation-owned tests/build plus clone-only RFB proof on the disposable desktop. Do not treat this review as that proof.
