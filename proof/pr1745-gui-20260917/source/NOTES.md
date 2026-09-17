# Source records

Alternate source UUID `d1cc3c01-c6a0-43a1-82c0-61b841f8f636` stayed **stopped**. Unrelated UUID `58b49202-a6de-4e24-ba8f-c1890c778535` stayed **suspended**. Other-host source `b98e38c2-2d06-403c-872f-6315bc68b461` is historically modified and was **not operated**.

`inventory-before.json` is that pair only (clone identity is `../readiness/clone-created.json`).

Initial full disk hashing (`disk-before.txt`, `16:58:16Z`–`17:01:22Z`) matches the morning baseline:

| HDS | SHA-256 |
| --- | --- |
| `{5fbaabe3-6958-40ff-92a7-860e329aab41}` | `2d3309308981eaf719b65a00d5572b37c16d804cff0117e381cb9a3ab67b88b2` |
| `{db2643d9-6201-49bd-90c0-5c3e3cc3f2e5}` | `aa725e3236e8132f1890daa20d671526795142cdeafab03444ecc2ca814b6ea0` |

`config-before.sha256`, `config-after-gui.sha256`, and `config-after-final-gui.sha256` are the same seven source/unrelated sidecars. After GUI and after the final-account RM grant they still match:

| Path token | SHA-256 |
| --- | --- |
| `$SOURCE_ROOT/config.pvs` | `87c20393d5567327f8546ab739d1fa3c545cd322fe6d07111cad9731472d00a7` |
| `$SOURCE_ROOT/Snapshots.xml` | `b98767343173f4f4df44e4ebad51ea05a545e4971db44baae67f8b9e6a577c08` |
| `$SOURCE_ROOT/aux.bin` | `7c228cdf7051cb2c9540a68bfa1f3e615a830d8dd82b5a25d94bfc95ed762e1d` |
| `$SOURCE_ROOT/machw.bin` | `976e66740c64041cf9f127588f93eabf7efb9e7d3cbaee9582bcded88ba5e8d3` |
| `$SOURCE_ROOT/macid.bin` | `232833db3b7dc2dd1c8a2008edf3d4b705164504e1ee5501ad13b429d342fa4b` |
| `$SOURCE_ROOT/harddisk.hdd/DiskDescriptor.xml` | `0b4da07a489464d73342e3607dc39deca785d13d76309af3847ea3c361a43bfa` |
| `$HOST_HOME/Parallels/unrelated-vm.macvm/config.pvs` | `34b94cf8867b25aff3f307cbd7d3dd677da0e2b62b42c4ab855e80c542d68189` |

## Guarded cleanup (filed)

See `../cleanup/`. `guarded-cleanup.json` `result=PASS` at `18:00:16Z` on binary `27c72217…`:

- inventory before release: clone **stopped**, source stopped, unrelated suspended
- SSH shutdown (`parent-clone-shutdown.txt`) closed the guest connection but inventory cycled starting/running (`final observed state: running`)
- parent then exact-UUID `prlctl stop --kill` (`parent-clone-exact-force-stop.txt`); clone independently stopped
- native `crabbox stop --provider parallels --target macos --id cbx_12f11bb0ab75` only after that independent stop
- script `exact_force_stop_used=false` / `exact_force_stop_enabled=false` refers to the **script**, not the parent. The parent did use exact force-stop before script cleanup.
- after: clone UUID/bundle/claim/key dir absent; source still stopped; unrelated still suspended
- task Peekaboo ended; no lease SSH remained (`task-process-cleanup.json`)
- no wildcard delete; source not operated

## Source-final AFTER

**PASS** at `2026-09-17T18:03:33Z`. Complete sanitized records are in `source-final/`.

`source-final-comparison.json`: `result=PASS`, `disk_before_after_equal=true`, `all_7_source_unrelated_config_hashes_match_original_baseline=true`, `hds=2`, `configs=7`. After-hashes equal before-hashes for both source HDS and the same seven sidecars. Post-cleanup inventory: source **stopped**, unrelated **suspended**, clone absent. Other-host source `b98e38c2-…` was not hashed and not operated.
