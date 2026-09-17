# Retained helper FAILs

These are **evidence-helper** classifier misses, not native `0095e1a3` product failures. Original helper receipts stay FAIL. Parent verdicts are separate and cite evidence hashes.

## 1. `prlctl exec` argv `--`

Native 0095 argv is `prlctl exec <uuid> <executable> [args…]` with **no** `--`. Helpers that insert `--` get `bash: --: invalid option` on this Apple VZ guest. Parent used the native argv (`exec <uuid> /bin/sh` + stdin) for operator fixture writes.

## 2. `commentNoReboot`

`# No reboot` lines in guest recipes are documentation. A helper substring scan treated them as live reboot and refused `--execute` after pubkey injection. Parent still applied the recipes. Guest did not need a product reboot for SSH/sudo/Node.

## 3. Overly-narrow unavailable-message whitelist

Live `prlctl exec` exit 255 text:

```
Unable to open new session in this virtual machine. Make sure your virtual machine has finished booting, runs the latest version of Parallels Tools, and is not isolated from the host OS.
```

Helpers that only admit `prl_err_vm_exec_guest_tool_not_available` / “guest tools are not available” false-FAIL frozen-audit and fresh-acquire even when SSH audit, blank Tools IPs, and `prltoolsd` absent/disabled already hold. Parent frozen gate and `fresh-native-verdict.json` use the raw text plus those audits.

Native `dhcp-mac` `prepareGuest` bootstraps SSH unconditionally; the native error classifier is unused on this path.

## 4. Follow-on `run` without the trusted bootstrap flag

Initial helper `run` omitted `--parallels-bootstrap-key` and timed out. `config show` reports `provider_source=repo_config`. A config file inside the task cwd **must not** select the host-side bootstrap identity. That ignore is a **designed trust boundary**.

The **acquire warmup already included the flag and passed**. The corrected `run` with the explicit flag passed (`20:22:25Z`, `sw_vers` 26.5.2). Do not rewrite history as “initial acquire failed.”

## 5. Fixture-delete helper

Parent exact-UUID `prlctl delete 5b2f2234-…` PASS. The closeout helper first refused a verdict field shape, then raised `AttributeError: 'Runner' object has no attribute 'run_exact_fixture_delete'`. Those helper FAILs are retained; they did not perform the delete.

## Do not publish

- Research claiming Node is unused on acquire. Guest and `sshReady` require Node/npm; this fixture shows `node=v24.19.0` / `npm=11.17.0`.
- Helper FIX notes claiming `sudo_n` missing. Frozen and post-acquire audits both print `sudo_n=ok`.
