# Fresh native acquire

Pinned binary `27c72217…` (`0095e1a3`). Isolated `CRABBOX_CONFIG` / empty `XDG_STATE_HOME` under `$FRESH_ROOT`. Source of the full clone was disposable fixture `5b2f2234-a4d2-4600-a7a3-a9c618599246` (stopped), **not** original `d1cc…`.

## Warmup (the acquire)

| Field | Value |
| --- | --- |
| Preflight UTC | `2026-09-17T20:20:50.744453Z` |
| Argv (trusted flag present) | `warmup --provider parallels --target macos --parallels-source-id 5b2f2234-… --parallels-clone-mode full --parallels-user parallels-02 --parallels-bootstrap-key $TASK_SECRETS/id_ed25519 --ssh-port 22 --keep --slug pr1745-fresh-dhcp` |
| Exit | 0 |
| Duration | 18.358s |
| Lease | `cbx_c19f90406ff5` |
| VM | `65f55c4d-2f6e-42eb-bc40-8910993486f7` |
| New MAC | `001c426e3c3b` |
| Fixture MAC | `001c42a811cf` |
| stderr | `discovery=dhcp-mac bootstrap=ssh` |
| Claim | `ip_source=dhcp-mac`, `sshPort=22`, `host=local` |
| DHCP | `10.211.55.14="1789678261,1800,001c426e3c3b,01001c426e3c3b"` at `20:23:09Z` |

Empty state before: no claim files, no key dirs, no reused prior leases (`cbx_12f11bb0ab75`, `cbx_406764ae8f7a`, `cbx_56d2cacc3552` not reused).

## No-Tools + lease key (after warmup)

Independent of the helper classifier:

- Tools IPs `[]`; GuestTools `not_installed`
- `prlctl exec <lease-uuid> /bin/sh -lc true` exit **255**, text exactly: `Unable to open new session in this virtual machine. Make sure your virtual machine has finished booting, runs the latest version of Parallels Tools, and is not isolated from the host OS.`
- SSH audit as `parallels-02` / uid 501, `sudo_n=ok`, Node `v24.19.0` / npm `11.17.0`
- `prltoolsd_print=absent` and `com.parallels.vm.prltoolsd => disabled`
- Native bootstrap **installed** the new lease key. The subsequent authenticated lease-key audit **observed** authorized keys **2**: bootstrap `SHA256:Gddbnk755iwVdqFqm1HGr9cZBmPm1XKLxX4h48EQFZo` + lease `SHA256:vBa6uzfQdc2XX9HHgkXhDw/AtMYjHlJbaV2dqwZmidI`. The audit did not install the lease key.
- Markers after native EnsureReady: `/usr/local/bin/crabbox-ready` present; `/var/lib/crabbox/bootstrapped` present; `$GUEST_HOME/crabbox-work` present; `/var/db/crabbox/bootstrapped` absent (0095 does not write that path)

## Follow-on `run`

The helper's first `crabbox run --id cbx_c19f90406ff5 -- /usr/bin/sw_vers` **omitted** `--parallels-bootstrap-key`. Exit 5 / timeout (`timed out waiting for Parallels VM … IP`). `config show` selected `provider_source=repo_config`; a config path inside cwd cannot select the trusted host-side bootstrap key. That is a **designed trust boundary**, not a failed acquire. The warmup that created the lease **did** pass the flag.

Parent then ran the same `run` with the explicit trusted flag at `20:22:25Z`: exit 0 in ~1.2s; `sw_vers` ProductVersion `26.5.2` Build `25F84`. No acquire rerun; no guest rewrite.

## Verdicts (do not collapse)

| Receipt | Result | Role |
| --- | --- | --- |
| Helper `fresh-acquire.json` | FAIL (unchanged; sha256 `fa195189…`) | classifier + omitted-flag `run` |
| Parent `fresh-native-verdict.json` | PASS `20:23:09Z` | independent assessment on evidence hashes |

Do **not** call the initial acquire failed.

## `config show`

Full all-providers dump omitted. Labeled excerpt: [config-show.excerpt.txt](config-show.excerpt.txt). Relevant facts: `provider=parallels`, `provider_source=repo_config`, `parallels … source_id=5b2f2234-… clone_mode=full`, `ssh=parallels-02@<host>:22`, `provider_status name=parallels selected=true`.
