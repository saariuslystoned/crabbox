# Source records

Alternate source UUID `d1cc3c01-c6a0-43a1-82c0-61b841f8f636` stayed **stopped**. Unrelated UUID `58b49202-a6de-4e24-ba8f-c1890c778535` stayed **suspended**. Other-host source `b98e38c2-2d06-403c-872f-6315bc68b461` is historically modified and was **not operated** this task. This packet has no write/power inventory of that other-host VM.

`source-baseline.txt` is the recorded inventory plus source config/Snapshots/aux/machw/macid/DiskDescriptor digests and the two 128 GiB HDS size/mtime rows. Unrelated config digest is kept; the unrelated display/bundle name is genericized.

Initial full disk hashing is **complete** for both HDS files. `source-disk-before.sha256` (`11:52:30Z`) and `source-disk-openssl-before.txt` (`11:48:54Z`) match:

| HDS | SHA-256 |
| --- | --- |
| `{5fbaabe3-6958-40ff-92a7-860e329aab41}` | `2d3309308981eaf719b65a00d5572b37c16d804cff0117e381cb9a3ab67b88b2` |
| `{db2643d9-6201-49bd-90c0-5c3e3cc3f2e5}` | `aa725e3236e8132f1890daa20d671526795142cdeafab03444ecc2ca814b6ea0` |

`source-disk-before.partial.sha256` is the earlier first-HDS-only capture of `2d330930...`. It is historical, not a pending-hash claim.

`source-config-after-reuse.sha256` and the seven `config_sha256` rows in `post-cleanup-audit.json` (`12:12:10Z`) both match that baseline set.

Post-cleanup OpenSSL AFTER hashes (`source-disk-after.txt`, `12:12:10Z`–`12:15:16Z`) match both before-hashes. `source-final-comparison.json` (`12:16:12Z`) records `disk_before_after_equal=true`, independent before-hash tools agree, sizes/mtimes match baseline, and `result=PASS`. That is alternate-source immutability for this task only. Other-host source `b98e38c2-...` has no byte-immutability claim.

`preflight-cleanup-confirmed.txt` is cleanup of exploratory clone `15829ace-273d-428d-8a4b-70042b67450b` only.

Guarded live-lease cleanup: first pass (`12:10:30Z`) shut the clone down, then **refused delete** because independent inventory never observed `stopped`. Retry (`12:11:49Z`–`12:11:50Z`) used exact-UUID forced stop, independently rechecked `stopped`, then native Crabbox `stop` of exact claim `cbx_406764ae8f7a` exited 0. After release: clone bundle, claim, lease-key dir, and lease metadata absent. Inventory is stopped source + suspended unrelated VM only. Cancelled-acquire material `cbx_56d2cacc3552` is also absent. Task-owned VM directory entries are empty.
