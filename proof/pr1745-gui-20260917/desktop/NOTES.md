# Desktop records — supported GUI, not kickstart

This is the fresh System Settings route. It does **not** use kickstart, TCC DB edits, or autologin. Disposable guest names `cbxgui1745`, `cbxproof1745`, and `cbxfinal1745` are publishable. Generated passwords are not in this packet.

`1982f4e8` partial type and `ff5d4cd5` blank type are **retained failed history**. The live pass is native `0095e1a3` on `cbxfinal1745`.

## 1. Parent binary exposed the old blocker

`gui-helper.txt` created native admin `cbxgui1745` UID 502 (`verify=ok`, `kickstart=not_invoked`, `argv_has_secret=0`). Native `03de7896` screenshot then failed before RFB:

```
lease cbx_12f11bb0ab75 was not created with desktop=true; warm a new lease with --desktop
crabbox.exit=2
```

Native acquire intentionally omitted `--desktop` (used `-all`). That refusal is the old desktop blocker. `1982f4e8` adds the fail-closed owned-clone allow path without forging `desktop=true`.

`rm-independent.txt` (`17:18:10Z`) shows console `cbxgui1745`, `naprivs=-2147483646`, `ARD_AllLocalUsers=0`, TCP 5900 open. First RM grant, later replaced.

## 2. Proof account RM (historical)

`gui-proof-helper.txt` created native admin `cbxproof1745` UID 503. Old `cbxgui1745` was **not** reset. Parent granted only Observe+Control to `cbxproof1745`, then removed `cbxgui1745` from RM (account retained).

Published frames: `proof-account-observe-control.png`, `proof-account-restricted.png` (VNC-viewer password row is masked bullets only; not a login/recovery capture).

`rm-proof-account-independent.txt` (`17:26:34Z`) still lists console `cbxgui1745`. That is **before** the allowlist swap; not the final grant.

## 3. Failed black-frame candidate — history, not success

The first same-session ARD-30 probe exited 0 with `frame_changed=1` / `input-effect-candidate=1`. Parent saw a **black** framebuffer except a tiny cursor. Treat as **FAILED**. Those PNGs are **not** published. Helper classification alone is not acceptance.

## 4. Proof-account visible nonce (still valid)

After a normal GUI logout of `cbxgui1745` and login of `cbxproof1745`:

- `proof-account-before-rfb.png` — preopened blank TextEdit, no nonce.
- RFB authenticated as `cbxproof1745` (ARD 30) and typed literal text. Spotlight launch is **not** proven.
- Parent visually read `PR1745_RFB_GUI_20260917_1730_CC38` in `proof-account-rfb.png` and `proof-account-rfb-independent.png`.

## 5. Native `1982f4e8` screenshot passed; type did not

`gui-proof-helper.txt` records `set-crabbox` of `$TASK_ROOT/crabbox-1982f4e8` and screenshot `crabbox.exit=0`. `crabbox-1982f4e8-screenshot.png` shows the RFB nonce already in the document.

Native type of `_CRABBOX_1982F4E8_1736` reported `bytes=22` exit 0. Independent evidence shows only a trailing `_`:

- `crabbox-1982f4e8-input-independent.png`
- `native-type-partial.txt` — text `PR1745_RFB_GUI_20260917_1730_CC38_`

**PARTIAL / NOT a native text pass.** Superseded by `0095e1a3`.

## 6. Final account `cbxfinal1745` — only RM Observe+Control

`gui-final-helper.txt` created native admin `cbxfinal1745` UID 504 (`kickstart=not_invoked`). Parent granted **only** Observe+Control in normal System Settings and removed `cbxproof1745` from the RM allowlist (account retained, not reset).

| File | Role |
| --- | --- |
| `final-account-observe-control.png` | Observe+Control on `cbxfinal1745`; every other privilege off |
| `final-account-restricted.png` | RM On; allowlist only `cbxfinal1745`. VNC password row is masked bullets. |

`final-account-permissions.txt` (`17:52:13Z`): console `cbxfinal1745`, `naprivs=-2147483646`, next account has no `naprivs`, `ARD_AllLocalUsers=0`, FileVault Off, and the helper nonce already in the guest document.

## 7. Same-session helper nonce (valid)

Same-session helper accepted ARD security 30 as `cbxfinal1745` and delivered entire `PR1745_FINAL_RFB_20260917_CC38`. Parent visually confirmed:

- `final-account-same-session-rfb.png`
- `final-account-same-session-independent.png`

Helper `input-effect-candidate` is still not acceptance; the frames are.

## 8. Native `ff5d4cd5` blank failure — history, not success

`gui-final-helper.txt` records `set-crabbox` `$TASK_ROOT/crabbox-ff5d4cd5`, then `typed bytes=33` exit 0 and a later `bytes=23` exit 0. Parent: blank TextEdit both times. Those frames are **not** published. Do not claim native type on `ff5d4cd5`. The later 2s settle exists because this run showed ready/drain alone was not enough.

## 9. Native `0095e1a3` actual pass

`gui-final-helper.txt` then `set-crabbox` `$TASK_ROOT/crabbox-0095e1a3` (binary `27c72217…`). Preopened empty `PR1745-native-0095e1a3.txt` (`native-0095-before.png`).

1. Native type `PR1745_NATIVE_0095E1A3_CC38` (27 bytes). Independent `native-0095-input-independent.png` shows the whole nonce.
2. Fresh CLI invocation `_SECOND_0095E1A3_OK` (19 bytes). Independent `native-0095-second-independent.png` shows the concat.
3. Native screenshot exit 0: `native-0095-screenshot.png` shows the same concat.
4. Later normal GUI File > Save (no text entered). Independent guest file `native-0095-file-independent.txt`:

```
PR1745_NATIVE_0095E1A3_CC38_SECOND_0095E1A3_OK
```

No competing console input during typing. Guest exec did not write a new nonce. Parent visually confirmed the independent frames. CLI byte count is not the proof; the images and file are.

The 2s cancel-aware settle is a documented delay, not an RFB/ARD handshake and not glyph proof.

## 10. What was never used

No TCC DB write, no autologin, no kickstart, no existing-password reset, no source/unrelated VM operation. Host screensaver/Peekaboo/node prompts stayed on the proof host and are **not** published (`host-*`, `current.png`, login / password-field / recovery / setup / onboarding captures excluded). FileVault is Off on the clone.
