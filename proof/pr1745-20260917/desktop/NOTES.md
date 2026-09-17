# Desktop records — failure, not success

Do **not** publish or infer account-authenticated RFB success, nonce entry, or desktop acceptance from this directory. Account desktop is **not proven**. This run stopped; completing desktop proof requires normal clone-local System Settings or MDM setup.

`desktop-attempt.txt` is the first captured helper result and stays historical. Staging of the guest script/JXA/public-key paths succeeded (`stage.exit=0`). Account setup then failed:

```
set-od-password: OpenDirectory password set failed
OpenDirectory password set failed (-2700)
account.exit=1
guest account setup failed
```

`desktop-attempt-b.txt` is the later retry. Staging succeeded (`odpasswd` helper, `stage.exit=0`). Account setup failed again before Remote Management / RFB:

```
od-error domain=com.apple.OpenDirectory code=5100
account.exit=1
guest account setup failed
```

`desktop-attempt-c.txt` is the native `sysadminctl` path. Staging succeeded. Account `cbxdesk1745c` UID 504 was created and password-verified (`verify=ok`, `auth_authority=present`, `argv_has_secret=0`, prompted `-password -`). Kickstart then failed:

```
Can't call method "print" on an undefined value at .../kickstart line 695.
account.exit=1
guest account setup failed
```

`desktop-system-policy-blocker.txt` (`12:10:06Z`) independently records kernel System Policy `deny(1) file-write-create` of `/Library/Application Support/Apple/Remote Desktop/RemoteManagement.launchd`. Remote Management was not enabled. No RFB session, no nonce, no Screen Sharing permission grant.

Generated passwords were transient and are not in this packet. Do not use source or existing `parallels-02` credentials.

`console-before.png` is a pre-RFB console capture of the then-live clone (lock/login screen, Thursday Sep 17 7:43, existing guest account `parallels-02`, password field visible). It is non-black. It is **not** an RFB framebuffer, **not** a nonce proof, and **not** desktop acceptance. There is no published `console-after.png` or RFB screenshot.
