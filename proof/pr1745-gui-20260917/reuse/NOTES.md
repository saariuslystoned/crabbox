# Reuse records

Authoritative run is `saved-port-final/` (`17:59:24Z`) on binary `27c722172163ac237a29a687957378a069f0ce3ab411c8e119920b48d36063c9`.

## Authoritative `saved-port-final/` — PASS + noTools

`saved-port-trace.json` `result=PASS`, `no_tools_achieved=true`, `service_loaded=false`, `service_disabled=true`, Tools IPs `[]`. GuestTools **package** may still be installed; that is informational, not a loaded service.

Independent facts:

- generic config port **2222** (`config-port-default.txt`: `ssh=parallels-02@<host>:2222`); claim `sshPort=22`; management user `parallels-02` (desktop users refused)
- TCP: claim host port **22 open / 2222 closed** (`independent-port-observation.json`)
- SSH `launchctl print` : service `com.parallels.vm.prltoolsd` not found (`prltoolsd-print.txt`)
- SSH `launchctl print-disabled`: `"com.parallels.vm.prltoolsd" => disabled` (`prltoolsd-print-disabled.txt`)
- native `crabbox run … --no-sync -- /usr/bin/sw_vers` exit 0: macOS `26.5.2` / `25F84` (`reuse-saved-port.txt`)
- `prlctl exec` was **not** used for the Tools probe
- FileVault Off; this script did not reboot (`reboot_implemented=false`). Parent had already done a clone-only Tools-clear reboot before the probe.

## Historical `saved-port/` — parser miss

The first live run (`17:57:30Z`) also had reuse PASS (`sw_vers` exit 0, same ports, Tools IP blank, service absent, raw `=> disabled`) but set `no_tools_achieved=false` because the parser required the token `true`/`false`. Keep that directory as the conservative miss. Do not treat it as the current noTools result.

A blank Tools IP alone is never noTools.
