# Native Stop from RUNNING

Exact lease `cbx_c19f90406ff5` / VM `65f55c4d-2f6e-42eb-bc40-8910993486f7`. Binary `27c72217…`.

| Field | Value |
| --- | --- |
| Inventory before | `20:23:09.965Z` — lease **running**; fixture stopped; source stopped; unrelated suspended |
| Native argv | `$PINNED_CBX stop --provider parallels --target macos --id cbx_c19f90406ff5` |
| Parent pre-kill | **false** |
| Guest halt | **false** |
| Native stop UTC | `2026-09-17T20:23:10.212Z` |
| Exit | 0 |
| Inventory after | `20:23:12.578Z` — lease UUID **absent**; fixture still stopped; source stopped; unrelated suspended |
| Absence | UUID, bundle, claim, key dir, lease metadata all absent |

stderr (retained in full):

```
warning: could not stop GitHub Actions hydration for cbx_c19f90406ff5: write GitHub Actions hydration stop marker on 10.211.55.14: exit status 255
deleted lease=cbx_c19f90406ff5 server=65f55c4d-2f6e-42eb-bc40-8910993486f7 name=crabbox-cbx-c19f90406ff5-pr1745-fresh-dhcp
```

The hydration-stop-marker 255 is **nonfatal**. No GitHub Actions were requested or configured.

## Qualification

This proves Crabbox's **implemented** Parallels Stop: `prlctl stop --kill` then delete, starting from independently `running`, with no parent `--kill` and no guest `shutdown`.

It does **not** prove graceful ACPI / `shutdown -h now` to independently `stopped`. Native Parallels Stop has been `--kill` since the provider landed; this PR did not change it. Pete's 19:06Z sentence correctly qualified the earlier GUI cleanup (parent force-stop first). That qualification is now answered for the **implemented** path. A graceful enhancement is a **separate** product change, not a 1745 feature or acceptance gate. See [../research/shutdown.md](../research/shutdown.md).
