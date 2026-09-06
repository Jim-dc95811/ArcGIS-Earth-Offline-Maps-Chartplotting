# Mobile Offline Field Notes

## What has been proven in this project

- A district-wide archive exceeding **300 GB** has been built at Z20 detail and used offline.
- The archive has been exercised on a Windows desktop, Windows laptop, Android phone, Android tablet, and Amazon Fire tablet.
- On an Android phone, TPKX files have opened from **internal storage**, **microSD**, and **exFAT USB storage**.

These are project test results, not a guarantee that every Android model or OS build will behave identically.

## ArcGIS Earth Mobile and local files

Esri documents TPK and TPKX as supported local offline file types in ArcGIS Earth Mobile. The project's practical finding is that removable storage may behave differently depending on how the file is handed to ArcGIS Earth.

### Reliable field pattern observed

If ArcGIS Earth's in-app file picker hangs or refuses removable storage, open the TPKX from Android's file manager and hand it to ArcGIS Earth. The same TPKX can still be read successfully.

## Storage planning

High-detail imagery consumes substantial storage. Keep the archive organized by production block and zoom, verify copied files before deployment, and treat the archive master as read-only working material whenever possible.
