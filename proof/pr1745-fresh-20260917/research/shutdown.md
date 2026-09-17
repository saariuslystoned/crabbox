# Shutdown recommendation (scoped)

Pete, 2026-09-17 19:06Z: cleanup of the GUI clone included a preceding exact-clone force-stop, so **native graceful shutdown alone is not proven**. He then named a *different* remaining coverage gap: fresh no-Tools acquire. He did not ask for a Stop rewrite and did not treat graceful shutdown as the remaining 1745 coverage gap.

## What this fresh packet adds

Native `crabbox stop --id cbx_c19f90406ff5` ran against an independently **RUNNING** exact clone with **no** parent `prlctl stop --kill` and **no** guest halt. Exit 0; UUID/bundle/claim/keys absent. That isolates Crabbox's **implemented** Parallels path.

## What it does not add

Native Parallels `Stop` has been `prlctl stop <id> --kill` since the provider landed (2026-05-21). Unchanged on `main` and unchanged by PR 1745. `Delete` force-stops only when state equals `running`, then deletes. Docs describe ownership-fenced clone delete, not ACPI / `prlctl stop` without `--kill`.

So:

- **Not** an unmet original acceptance item (Sept 4 asked for independent cleanup of the task-owned clone with source/unrelated untouched).
- **Not** a defect in this PR's shutdown/release code.
- **Not** graceful guest power-off to independently `stopped`.

## Recommendation

Our recommendation — not imposed maintainer policy — is to keep graceful-then-kill **out of 1745** as an optional later follow-up. A graceful enhancement would be new Parallels product behavior on main, not a repair of this PR. Native Stop from RUNNING with no pre-kill was tested as the implemented `--kill`+delete path. Nothing graceful is proven. If someone later wants ACPI / `shutdown -h now` proof, that is a separate experiment (Tools still loaded, long poll). This packet should not be read as requesting or accepting that change.

The remaining Pete-named **implementation** item that this packet does not close is the generic capability-gate design, not Stop.
