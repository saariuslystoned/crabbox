# Readiness records

`acquire-ssh.txt` is native warmup on the **initial `03de7896` binary**, not a fresh `0095e1a3` acquire. Desktop type, saved-port/noTools, and guarded cleanup later used `0095e1a3` / `27c72217…` against this same claim. Host is `local` in the later claim. It records:

- source `d1cc3c01-c6a0-43a1-82c0-61b841f8f636`
- `clone_mode=full`
- lease `cbx_12f11bb0ab75`
- VM `cc38d3ad-2e77-4aa1-b83c-65b3cc145ea0`
- guest `parallels-02@$GUEST_IP:22`
- configured generic port 2222 closed; saved/auth port 22 used
- `warmup complete` / JSON `exitCode=0` in 40.06s

Native Crabbox bootstrap in this log is SSH ready-check / lease-key preparation on the **clone**. It is **not** automatic Node or npm installation, and it is **not** `--desktop`.

The source image lacked Node. After native SSH preparation, the operator installed official Node `22.23.1` / npm `10.9.8` **on this clone only**. `clone-node-prerequisite.txt` records the official tarball checksum OK and those versions.

`claim-before-gui.json` is the exact local claim before GUI account work: `sshPort=22`, `labels.host=local`, `cloudID=cc38d3ad-...`, `claimedAt=2026-09-17T16:59:04Z`. Isolated config/state/keys stay under `$E2E_ROOT`. `labels.ip_source=tools` is acquire-time metadata, not a later noTools claim.

`clone-created.json` is sanitized `prlctl` info for the owned clone only. `inventory-before.json` is source stopped + unrelated suspended, captured before GUI account work.

`ssh.cmd.native.txt` is the product-shaped native argv (multiplex flags present). `ssh.cmd.raw.txt` is the task-runtime file after prepending `ControlMaster=no`, `ControlPersist=no`, `ControlPath=none` so the proof SSH tunnel would not inherit a broken multiplex. Same key/guest/port. Product SSH and the claim were not changed.

Later saved-port reuse and guarded cleanup used this same claim shape (`parallels-02` @ `$GUEST_IP`:22). The lease is now released; see `../reuse/NOTES.md` and `../cleanup/`.
