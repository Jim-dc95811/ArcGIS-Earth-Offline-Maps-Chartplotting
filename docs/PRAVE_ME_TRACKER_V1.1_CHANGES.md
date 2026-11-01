# PRAVE / ME Tracker v1.1 - change document

**Released:** 2026-10-31
**Type:** hardening release. Packaging, reconnect behavior, error handling and logging.
**Download:** [`downloads/PRAVE_ME_Tracker_v1.zip`](../downloads/PRAVE_ME_Tracker_v1.zip) (the file name is unchanged so existing links keep working; it now contains v1.1).

**The Gold decoding and display path is unchanged.** RMC and PRAVE checksum validation, field positions, coordinate conversion, district + 3-digit IDs, RSSI icon levels, the ArcGIS Earth drawing payloads and ME camera tracking (height, heading, tilt and roll preserved) are identical to v1.0. The user settings above HERE BE DRAGONS are unchanged.

## What changed

### 1. The launchers work again

**v1.0 download:** `START_TRACKER.bat` and `SELF_TEST.bat` in the published zip were each saved as a single line containing the literal text `\n` instead of real line breaks. Windows could not run them, so the downloaded tracker could not be started from either launcher.

**v1.1:** both files are rebuilt with normal Windows line breaks. Their commands are exactly the ones intended.

### 2. A serial reconnect no longer clears the map

**v1.0:** every reconnect after a serial problem (USB cable pulled, radio power blip, COM port reset) re-ran the startup cleanup. It deleted every PRAVE unit drawing from ArcGIS Earth and deleted the saved unit list. Each truck reappeared only when its next PRAVE report arrived.

**v1.1:** old drawings are cleaned once, when the tracker starts. A reconnect leaves every truck at its last reported position and keeps the unit list, so the next startup still cleans them up. Session counters also carry through a reconnect.

### 3. Missing pyserial is reported once

**v1.0:** without pyserial the tracker printed `ERROR pyserial is not installed` every 5 seconds forever.

**v1.1:** the message is shown once and the tracker exits with code 1. A missing or busy COM port still retries every 5 seconds as before, since the radio may be plugged in later.

### 4. Icon problems are visible

**v1.0:** if the RSSI icon folder was missing, units silently showed ArcGIS Earth's default marker. A single missing icon file produced a warning on every report from that unit.

**v1.1:** at startup the tracker reports a missing icon folder, or lists any missing icon files. The per-level fallback warning appears once.

### 5. Cleaner diagnostics

- An ArcGIS Earth Automation API failure is logged as `API_BAD` only. v1.0 also logged it as `RMC_BAD` / `PRAVE_BAD` and counted it as a bad sentence, which made a healthy radio feed look faulty.
- A GPS without a fix (RMC status `V`) is logged once as `GPS_NO_FIX` and once as `GPS_FIX_RESTORED` when the fix returns. v1.0 logged an `RMC_BAD` line every second while the fix was missing. The status line shows the count as `NO_FIX_RMC=`.
- Ctrl+C while the tracker is waiting to reconnect now stops it cleanly instead of showing a Python traceback.
- The banner shows `v1.1`.

## Verification

Testing used Jim Gaddy's own simulator, **PRAVE / RMC Field Decoder Tester v1.0 Gold**, unmodified: one `$GPRMC` per second plus six remote units, 7-101 to 7-106, one per RSSI level. The simulator fed a virtual serial port read by the tracker. A local stand-in for the ArcGIS Earth Automation API recorded every drawing and camera change. v1.0 and v1.1 ran the same scenarios.

| Scenario | v1.0 | v1.1 |
|---|---|---|
| Six units: position, label and RSSI icon (unknown, 1-5 bars) | correct | identical to v1.0 |
| ME drawing and camera centring; height, heading and tilt kept | correct | identical to v1.0 |
| Serial cable pulled and reconnected | 6 trucks -> 0; unit list deleted | all 6 kept; unit list kept |
| Drawings left by a previous session | removed at startup | removed at startup |
| ArcGIS Earth API returning errors for 4 s | 11 bad-sentence log lines | none; drawings recovered |
| GPS without a fix for 6 s | 5 `RMC_BAD` lines | 1 `GPS_NO_FIX`, 1 `GPS_FIX_RESTORED` |
| pyserial not installed | repeated forever | one message, exit 1 |
| Icon folder missing | no warning | one startup warning |
| Ctrl+C during reconnect wait | traceback | clean stop |

Also checked:
- The built-in `--self-test` passes.
- All Gold parsing, drawing and camera functions are unchanged from v1.0, compared function by function.
- A 60-second simulation at 19,200 baud confirmed that no sentences are split or lost by the serial read timeout.

**Live status:** Jim started v1.1 on Windows on 2026-10-31 and it came up normally. A full field test with the Raveon radio and ArcGIS Earth, including the unplug/replug check, is still to be done.

## Checksums

| File | v1.1 SHA-256 |
|---|---|
| `PRAVE_ME_Tracker.py` | `c1f2416c037dae7fada6b930b008a092290cc088b98b28f56936ab1ce27a23c4` |
| `START_TRACKER.bat` | `2a7a5de6c432c2deb5eff588742a25acb50e1dcafb78b90b0401c81eca817039` |
| `SELF_TEST.bat` | `d9765eed1a0b0285d3d31d3aab4dfb798e8ea537c197047a074eb80bc1dfa66b` |
| `PRAVE_ME_Tracker_v1.zip` | `43ef8146bb3f4a6a23daa44b4d4a31e410de97eb7c31136078b114015f79f32d` |

v1.0 `PRAVE_ME_Tracker.py` SHA-256 for reference: `dca19a7e9a835f9ce4f94ab724a6e986f3d237b67fd133f6927694644ca2a0dd`. v1.0 remains available in the repository history.
