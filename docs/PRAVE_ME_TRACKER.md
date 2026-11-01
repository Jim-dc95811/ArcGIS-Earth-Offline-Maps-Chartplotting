# PRAVE / ME Tracker Guide

**Current version: v1.1** (2026-10-31). See [PRAVE_ME_TRACKER_V1.1_CHANGES.md](PRAVE_ME_TRACKER_V1.1_CHANGES.md) for what changed from v1.0.

The PRAVE / ME Tracker replaces the older KML/NetworkLink display path with direct ArcGIS Earth Automation API control.

## Data path

```text
$GPRMC / $GNRMC -> checksum -> ME position -> ArcGIS Earth drawing -> camera center
$PRAVE          -> checksum -> remote position/ID/RSSI -> ArcGIS Earth drawing
```

The decoder preserves the proven PRAVE field layout used by the earlier Gold field decoder: latitude at field 3, longitude at field 4, district at field 8, RSSI at field 12, and individual ID at field 13. Display IDs use `district-individual`, with the individual padded to three digits.

## Why the camera tracking works

The tracker reads ArcGIS Earth's current camera, replaces only the camera X/Y with the current ME longitude/latitude, and writes the camera back. Camera height, heading, tilt, and roll are preserved. In live testing this kept ME centered without the aggressive zoom changes seen with KML Fly to View.

## Requirements

- Windows 10/11
- ArcGIS Earth desktop
- ArcGIS Earth Automation API enabled
- Python 3
- `pyserial`
- Mixed Raveon RMC/PRAVE serial input

## ArcGIS Earth setup

1. Open **Settings**.
2. Open **Advanced application settings**.
3. Enable **Automation API**.
4. Leave the default local endpoint at `http://localhost:8000` unless you intentionally changed it.

## Tracker setup

1. Extract `PRAVE_ME_Tracker_v1.zip`, then open `PRAVE_ME_Tracker.py` in the extracted folder.
2. Change only `INPUT_PORT` and, if necessary, `INPUT_BAUD` above the `HERE BE DRAGONS` line. If your RSSI icons live somewhere other than `C:\MyData\PRAVE_ME\Icons`, also set `ICON_FOLDER`.
3. Run `SELF_TEST.bat`.
4. Connect the radio/test source and run `START_TRACKER.bat`.

## RSSI icons

The six fire-truck PNGs are **not included** in the download. Place your own icons in `ICON_FOLDER` with these names: `firetruck_rssi_unknown.png`, `firetruck_rssi_1.png` through `firetruck_rssi_5.png`. At startup the tracker warns if the folder or any icon is missing. Units still appear with ArcGIS Earth's default marker until the icons are in place.

## ME tracking control

ME tracking starts enabled. Press **T** in the tracker console to toggle ME drawing replacement and camera recentering off/on.

## PRAVE drawing behavior

The ArcGIS Earth Drawings API provides add/delete operations for the point drawing path used here, so fresh PRAVE reports replace the previous drawing. If an individual PRAVE unit is unchecked in ArcGIS Earth while the tracker is running, the next report can recreate it as visible. Stop the tracker to suspend all updates.

## Reconnect behavior

If the serial link drops (USB cable pulled, radio power blip), the tracker keeps running and retries every 5 seconds. Units stay on the map at their last reported positions while it reconnects. Drawings left over from an earlier session are cleared only when the tracker starts.

## Log messages

Logs are written to the `Logs` folder beside the tracker. Useful entries:

- `API_BAD` - ArcGIS Earth did not accept an update; check that it is running with the Automation API enabled.
- `RMC_BAD` / `PRAVE_BAD` - a sentence failed its checksum or field checks.
- `GPS_NO_FIX` / `GPS_FIX_RESTORED` - the GPS lost and regained its fix.
- `ICON_WARNING` - the icon folder or an icon file is missing.
