# PRAVE / ME Tracker Guide

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
2. Change only `INPUT_PORT` and, if necessary, `INPUT_BAUD` above the `HERE BE DRAGONS` line.
3. Run `SELF_TEST.bat`.
4. Connect the radio/test source and run `START_TRACKER.bat`.

## ME tracking control

ME tracking starts enabled. Press **T** in the tracker console to toggle ME drawing replacement and camera recentering off/on.

## PRAVE drawing behavior

The ArcGIS Earth Drawings API provides add/delete operations for the point drawing path used here, so fresh PRAVE reports replace the previous drawing. If an individual PRAVE unit is unchecked in ArcGIS Earth while the tracker is running, the next report can recreate it as visible. Stop the tracker to suspend all updates.
