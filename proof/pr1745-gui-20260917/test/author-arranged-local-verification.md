# Author-arranged local verification

This is **not** current-head CI and **not** maintainer acceptance. Full `./...` is **not** claimed green.

## Scoped checks (implementation agents; this packet did not rerun)

| Head | Scope (parent/review-stated) | Result |
| --- | --- | --- |
| `1982f4e8` | owned-clone capability tests | pass |
| rebase `fab0aee2` on `f288eb7a` | `go vet` parallels/cli; `go test -race` parallels + filtered cli; `go build` | pass |
| `ff5d4cd5` | `go test -race` filtered cli RFB/Capabilit/Parallels/ChildEnv; parallels package; `go vet ./...`; `go build` | pass |
| `0095e1a3` | `gofmt`; `go vet ./internal/cli/`; `go test` (+race) `TestTypeRFBText\|TestRFBInputReadySettle\|…` | pass |

Independent reviews did **not** rerun those tests. `readiness-independent-review.md` and `final-integration-review.md` say so explicitly.

## Prior `03de7896` full suite (not this commit, not green)

| Check | Exit |
| --- | --- |
| `go test -race -timeout=10m ./internal/providers/parallels/` | 0 |
| focused `./internal/cli/` Parallels + help-contract filter | 0 |
| `go vet ./...` | 0 |
| `go build -trimpath -o bin/crabbox ./cmd/crabbox` | 0 |
| `go test -race -timeout=20m ./...` | 1 |

Hosted current-head CI for `0095e1a3` is four `action_required` workflows (`../review/final-workflows.json`: CI, Docs UI, Release Check, Connector E2E). Dispatch success is not CI. Full suite was not rerun on `0095e1a3`. Maintainer approval remains a separate gate.
