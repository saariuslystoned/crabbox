# Reuse records

Exact claim `reuse/exact-claim-before-configuration.json` was captured before extra clone configuration:

- `leaseID=cbx_406764ae8f7a`
- `cloudID=f3f156aa-93c3-4656-848c-05d472a57916`
- `labels.host=local`
- `sshPort=22`
- `claimedAt=2026-09-17T11:43:50Z`
- `labels.ip_source=tools`

`labels.ip_source=tools` is acquire-time claim metadata from before later Tools disable. This packet does not re-label the acquire as DHCP-MAC fallback.

`config-port-default.txt` shows generic **2222** and no `sshPort` override. `reuse-command-trace.txt` (`11:59:39Z`–`11:59:41Z`) is the combined reuse proof: config default 2222, exact claim `sshPort=22`, Tools `ipAddresses=[]`, independent port 22 open / 2222 closed, then a fresh CLI `run` with no `--ssh-port` printing `macOS 26.5.2` / `25F84`, exit 0.

`reuse-saved-port.txt` is the earlier saved-port `sw_vers` run (`parallels-02@10.211.55.11:22`, same product/build, exit 0). After the claim, Tools were disabled on **this clone only**. `clone-no-tools.json` then shows `GuestTools.state=installed` (package still present) and `Network.ipAddresses=[]`. `independent-port-observation.json` at `2026-09-17T11:44:56.667157Z` records VM `f3f156aa-...`, IP `10.211.55.11`, port 22 open, port 2222 closed.
