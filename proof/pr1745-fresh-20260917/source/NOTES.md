# Original source lineage (untouched)

Original source UUID `d1cc3c01-c6a0-43a1-82c0-61b841f8f636` stayed **stopped**. Unrelated UUID `58b49202-a6de-4e24-ba8f-c1890c778535` stayed **suspended**. Other-host source `b98e38c2-2d06-403c-872f-6315bc68b461` was **not operated**.

This is a **different object** from disposable fixture `5b2f…`. Fixture hashes live under [../fixture/](../fixture/NOTES.md). Do not mix the two.

Published files here are **sanitized excerpts**. Embedded `original_record_sha256` / `source_final_sha256` values are SHA-256 of the local **unsanitized** bytes. [MANIFEST.sha256](../MANIFEST.sha256) hashes these sanitized files and need not match after path redaction.

## Source-before (start of this task)

Readonly compare at `2026-09-17T19:59:06Z` against the prior GUI packet baseline. **PASS**: both HDS and all 7 source/unrelated configs match. Inventory: source stopped, unrelated suspended.

| Relpath | SHA-256 |
| --- | --- |
| `$SOURCE_BUNDLE/harddisk.hdd/harddisk.hdd.0.{5fbaabe3-6958-40ff-92a7-860e329aab41}.hds` | `2d3309308981eaf719b65a00d5572b37c16d804cff0117e381cb9a3ab67b88b2` |
| `$SOURCE_BUNDLE/harddisk.hdd/harddisk.hdd.0.{db2643d9-6201-49bd-90c0-5c3e3cc3f2e5}.hds` | `aa725e3236e8132f1890daa20d671526795142cdeafab03444ecc2ca814b6ea0` |
| `$SOURCE_BUNDLE/config.pvs` | `87c20393d5567327f8546ab739d1fa3c545cd322fe6d07111cad9731472d00a7` |
| `$SOURCE_BUNDLE/Snapshots.xml` | `b98767343173f4f4df44e4ebad51ea05a545e4971db44baae67f8b9e6a577c08` |
| `$SOURCE_BUNDLE/aux.bin` | `7c228cdf7051cb2c9540a68bfa1f3e615a830d8dd82b5a25d94bfc95ed762e1d` |
| `$SOURCE_BUNDLE/machw.bin` | `976e66740c64041cf9f127588f93eabf7efb9e7d3cbaee9582bcded88ba5e8d3` |
| `$SOURCE_BUNDLE/macid.bin` | `232833db3b7dc2dd1c8a2008edf3d4b705164504e1ee5501ad13b429d342fa4b` |
| `$SOURCE_BUNDLE/harddisk.hdd/DiskDescriptor.xml` | `0b4da07a489464d73342e3607dc39deca785d13d76309af3847ea3c361a43bfa` |
| `$UNRELATED_BUNDLE/config.pvs` | `34b94cf8867b25aff3f307cbd7d3dd677da0e2b62b42c4ab855e80c542d68189` |

These match the GUI packet source-final AFTER (`18:03:33Z`) and that packet's before baseline.

## Source-final AFTER this fresh task

**PASS** at `2026-09-17T20:30:45.991322Z`. Sanitized records: [source-final-comparison.json](source-final-comparison.json), [source-disk-after.txt](source-disk-after.txt), [source-config-after.sha256](source-config-after.sha256), [inventory-after.txt](inventory-after.txt).

`result=PASS`, `disk_before_after_equal=true`, `all_7_source_unrelated_config_hashes_match_original_baseline=true`, `hds=2`, `configs=7`. After-hashes equal the `19:59:06Z` before hashes for both HDS and the same seven sidecars. Inventory after: source **stopped**, unrelated **suspended**. Other-host `b98e38c2-…` was not hashed and not operated.

## Closeout

**PASS** at `2026-09-17T20:31:27.361809Z`. Sanitized record: [final-closeout.json](final-closeout.json). Only source stopped / unrelated suspended remain. Task fixture + lease bundles, keys, and known_hosts absent. `remaining_task_processes` is `[]`.
