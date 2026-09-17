# Readiness records

`acquire-ssh.txt` is native final-head warmup on the Parallels host (`host=local` in the later claim). It records:

- source `d1cc3c01-c6a0-43a1-82c0-61b841f8f636`
- `clone_mode=full`
- lease `cbx_406764ae8f7a`
- VM `f3f156aa-93c3-4656-848c-05d472a57916`
- guest `parallels-02@10.211.55.11:22`
- `warmup complete` / JSON `exitCode=0`

Native Crabbox bootstrap in this log is SSH ready-check / lease-key preparation on the **clone**. It is **not** automatic Node or npm installation.

The source image lacked Node. After native SSH preparation, the operator installed official Node `22.23.1` / npm `10.9.8` **on this clone only**, then warmup readiness completed. That install is an operator prerequisite for clone-local Node/npm, not Crabbox bootstrap and not a source-image change. `clone-node-prerequisite.txt` records those versions and `operator_clone_only_node_install_exit=0`.

`config-port-default.txt` is the isolated task config used for reuse: generic SSH port **2222**, `auth=missing` (no password in config), bootstrap identity path only (`$HOST_HOME/.ssh/id_ed25519`), no `sshPort` override. Extra clone-only admin after the exact claim (temporary NOPASSWD sudoers, disable `com.parallels.vm.prltoolsd`, clone reboot) is operator proof administration, not acquire bootstrap.

A prior cancelled acquire `cbx_56d2cacc3552` / `48d5af14-...` is not published here.
