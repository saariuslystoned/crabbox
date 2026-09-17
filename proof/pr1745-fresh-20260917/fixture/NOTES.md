# Disposable fixture (not the original source)

Fixture `5b2f2234-a4d2-4600-a7a3-a9c618599246` / `crabbox-pr1745-fresh-fixture-20260917` is a **parent-owned disposable full clone** of original source `d1cc3c01-…`. It is not a Crabbox lease. Name starts with `crabbox-` but has no claim. Never `crabbox stop --reclaim`. Never global cleanup.

Original source stays a separate immutable lineage: [../source/NOTES.md](../source/NOTES.md).

## Operator prerequisites only (not native bootstrap)

Parent applied pre-reviewed guest recipes via native `prlctl exec <uuid> /bin/sh` stdin (no `--`). Helpers that insert `--` or treat `# No reboot` comments as live reboot commands are retained FAILs; they did not perform these writes.

| Step | Result |
| --- | --- |
| Task bootstrap key | host-generated; **private bytes never published**; public FP `SHA256:Gddbnk755iwVdqFqm1HGr9cZBmPm1XKLxX4h48EQFZo` |
| SSH | enabled; stock user `parallels-02` |
| sudo | NOPASSWD fragment installed; guest `sudo_n=ok` |
| Node | official pinned **24.19.0** / npm **11.17.0** (sshReady prerequisite; not a Crabbox marker) |
| Tools | `launchctl disable system/com.parallels.vm.prltoolsd`; then stop/start |
| Product markers before acquire | all four absent (`crabbox-ready`, `/var/lib/crabbox/bootstrapped`, `/var/db/crabbox/bootstrapped`, `$GUEST_HOME/crabbox-work`) |
| Authorized keys before acquire | **1** (bootstrap only) |

## Frozen audit

Helper `frozen-audit.json` is **FAIL** solely because the exec-unavailable whitelist omitted the observed platform wording. Raw exec 255 text is retained.

Parent `frozen-before-parent.json` at `20:16:19Z` is **PASS**: Tools unavailable, service absent/disabled, SSH authenticated, task state empty, bootstrap key only, all product markers absent. Use that gate, not the helper result.

## Freeze-before hashes (fixture immutability, separate from source)

Fixture independently stopped (`prlctl stop --kill`) at `20:16:19Z` before hashing. Freeze-before `20:16:37Z`–`20:19:44Z` **PASS**: 2 HDS + 6 configs.

| Relpath | SHA-256 | Bytes |
| --- | --- | --- |
| `harddisk.hdd/harddisk.hdd.0.{5fbaabe3-…}.hds` | `bdecfccd3d57143d48c8369e99f505dd119f2695b8b1bb2bf5b65e679e3c9bd9` | 137438953472 |
| `harddisk.hdd/harddisk.hdd.0.{db2643d9-…}.hds` | `aa725e3236e8132f1890daa20d671526795142cdeafab03444ecc2ca814b6ea0` | 137438953472 |
| `config.pvs` | `1d79a1e777d5913c2fa0f6d84245ede6784a6ec166ead183adfbb2a5e970a25b` | 28744 |
| `Snapshots.xml` | `b98767343173f4f4df44e4ebad51ea05a545e4971db44baae67f8b9e6a577c08` | 927 |
| `aux.bin` | `d4df08ae73d075a4d051dbbe292379a44d6846354d43154f5fd50d6088478933` | 33579164 |
| `machw.bin` | `976e66740c64041cf9f127588f93eabf7efb9e7d3cbaee9582bcded88ba5e8d3` | 132 |
| `macid.bin` | `232833db3b7dc2dd1c8a2008edf3d4b705164504e1ee5501ad13b429d342fa4b` | 68 |
| `harddisk.hdd/DiskDescriptor.xml` | `39fb0ca3fabc9183fcb9ed8687b0797eeaee0b311364369f817581b5e854bd2f` | 2340 |

The first HDS differs from original source `2d330930…` because this fixture received operator guest writes. That is expected and **does not** mean the original source moved.

## Freeze-after hashes

After-hash `20:21:50Z`–`20:24:57Z` **PASS**. Parent verdict `20:27:16Z` **PASS**: all 8 paths equal freeze-before (`before_record_sha256=b9a3c87a…`, `after_record_sha256=1aea06f9…`). Original source hashes are unchanged and separate.

## Exact fixture delete

Parent `20:27:49Z` **PASS**. `prlctl delete 5b2f2234-…` exit 0 (`The VM has been successfully removed.`). After: fixture UUID/bundle absent; lease already absent; source stopped; unrelated suspended. Task bootstrap private/pub and known_hosts unlinked (paths only; private bytes never published). Helper delete attempts FAIL (verdict-shape refuse, then missing `run_exact_fixture_delete`); parent did the delete. No wildcard / reclaim / source operation. Closeout `20:31:27Z` independently confirms both disposables gone; see [../source/final-closeout.json](../source/final-closeout.json).
