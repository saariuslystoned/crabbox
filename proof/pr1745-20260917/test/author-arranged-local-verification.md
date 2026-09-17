# Author-arranged local verification

This is **not** current-head CI and **not** maintainer acceptance. Values are copied from the author-arranged Cursor implementation report against SHA `03de7896dd094e5feb7c74d480ce3995d2c8760f` / binary `7db4eea83126f9f888d7a491e29be7eb01cfd506633c322e3da4840f01639cc6`.

## Focused checks

| Check | Exit |
| --- | --- |
| `go test -race -timeout=10m ./internal/providers/parallels/` | 0 |
| focused `./internal/cli/` Parallels + help-contract filter | 0 |
| `go vet ./...` | 0 |
| `go build -trimpath -o bin/crabbox ./cmd/crabbox` | 0 |
| `go test -race -timeout=20m ./...` | 1 |

Feature-local `internal/providers/parallels` and `cmd/crabbox` passed inside the full run. Help-contract delta versus a pristine `upstream/main` archive was only `--parallels-bootstrap-key`.

## Full-suite failures (not masked)

Host `umask=077`. The same CLI and apple-machine failures reproduced on a pristine `upstream/main` archive. Do not treat them as PR 1745 regressions, and do **not** treat the suite as green. Load/timeout failures that passed isolated on both trees are listed in `review/cursor-implementation-report.md`.

Current-head GitHub CI remains `action_required` on **4** workflow runs (`review/current-head-workflows.json`). The earlier filed rollup is `review/current-head-ci.json`. That gate is independent of this host run.
