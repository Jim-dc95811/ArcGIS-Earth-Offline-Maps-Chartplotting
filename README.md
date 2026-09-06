# ArcGIS Earth Offline Maps & Field Tracking

> **Build high-detail maps once. Carry them offline. Keep the field display useful when the network is gone.**

This repository packages a practical offline-map workflow around **ArcGIS Pro**, **TPKX tile packages**, **ArcGIS Earth**, and an optional **PRAVE / ME live field tracker**.

The project has moved well past a one-machine proof of concept. A **300+ GB district-wide Z20 archive** has been built and exercised across Windows desktops/laptops, Android phones/tablets, and an Amazon Fire tablet. On Android, TPKX files have also been opened from internal storage, microSD, and exFAT USB media.

## The three tools

| Tool | Job | Download |
|---|---|---|
| **TPKX Extent Tool v2** | Build aligned EPSG:3857 parent, neighbor, 24-box, and 10-box production extents. | [ZIP](downloads/TPKX_Extent_Tool_v2.zip) |
| **ArcGIS TPKX Batch Factory** | ArcGIS Pro add-in that turns 1-10 controlled extents into Satellite, Road Overlay, or Both TPKX packages at Z16-Z20. | [ZIP](downloads/TPKX_Batch_Factory_v1.zip) |
| **PRAVE / ME Tracker** | Reads mixed RMC/PRAVE serial data and draws/tracks ME and remote units directly in ArcGIS Earth through the local Automation API. | [ZIP](downloads/PRAVE_ME_Tracker_v1.zip) |

## Workflow

```text
GPS points / known extent
          |
          v
   TPKX Extent Tool
          |
          v
  24-box or 10-box grids
          |
          v
ArcGIS TPKX Batch Factory
          |
          v
     finished .tpkx
          |
     +----+--------------------+
     |                         |
 Windows ArcGIS Earth     ArcGIS Earth Mobile
                               |
                      internal / microSD / USB
```

The live tracker is optional and sits beside the offline imagery workflow:

```text
Raveon mixed serial -> RMC = ME -> ArcGIS Earth camera center
                    -> PRAVE    -> remote units + RSSI drawings
```

```mermaid
flowchart LR
    A[GPS points / known extent] --> B[TPKX Extent Tool]
    B --> C[ArcGIS TPKX Batch Factory]
    C --> D[Finished TPKX archive]
    D --> E[Windows ArcGIS Earth]
    D --> F[ArcGIS Earth Mobile]
    F --> G[Internal / microSD / USB]
```

## Start here

1. Read the [end-to-end workflow](docs/WORKFLOW.md).
2. Build or load your production extents with the [TPKX Extent Tool guide](docs/TPKX_EXTENT_TOOL.md).
3. Produce TPKX files with the [Batch Factory operator guide](docs/TPKX_BATCH_FACTORY.md).
4. Review the [mobile offline field notes](docs/MOBILE_OFFLINE.md).
5. If you use Raveon PRAVE/RMC data, see the [PRAVE / ME Tracker guide](docs/PRAVE_ME_TRACKER.md).

## Manuals

- [TPKX Extent Tool v2 - Guide](docs/TPKX_EXTENT_TOOL.md)
- [ArcGIS TPKX Batch Factory - Operator Guide](docs/TPKX_BATCH_FACTORY.md)

The Extent Tool is intentionally only an extent generator; it does not download imagery or create TPKX files itself. The Factory is the production engine that consumes those extent lines. The manuals describe the same split: the 24-box grid handles controlled production, while the 10-box 2 x 5 cut is the project-standard Z20 batch size.

## What is field-proven here

- District-wide Z20 offline imagery archive exceeding 300 GB.
- Windows desktop and laptop use.
- Android phone and tablet use.
- Amazon Fire tablet use.
- TPKX opened from Android internal storage, microSD, and exFAT USB storage.
- ArcGIS Earth desktop ME center-screen tracking through the local Automation API while preserving the operator's selected zoom.
- PRAVE unit plotting with unit labels and RSSI-driven icon states.

Project-tested behavior is described as such; it should not be read as a guarantee for every Android OEM, ArcGIS version, storage device, or radio configuration.

## Requirements at a glance

### Extent Tool
- Windows
- Python 3
- Windows Clipboard History only if using the two-GPS-point helper

### Batch Factory
- Windows
- ArcGIS Pro
- .NET/ArcGIS Pro SDK build components used by the included installer
- Internet access **during production** to retrieve the selected tile service
- Sufficient local storage for high-detail TPKX output

### PRAVE / ME Tracker
- Windows
- ArcGIS Earth desktop
- ArcGIS Earth Automation API enabled
- Python 3 + `pyserial`
- RMC/PRAVE serial source

## Offline does not mean unlicensed

This repository contains workflow code and documentation, **not a redistributed imagery archive**. Users are responsible for the licensing, export permissions, and terms that apply to the map services/data they use to create TPKX packages.

## Official ArcGIS references

See [Official Esri References](docs/OFFICIAL_ESRI_LINKS.md) for the ArcGIS Earth Automation API, local-file support, and ArcGIS Pro offline tile-cache documentation.

## Project status

The three published ZIPs are cleaned project distributions built from the current field-tested packages supplied for this repository. Development/test-era names and cache files were removed where they were not operationally required. The downloadable ZIPs contain the program source and launch/build files; the repository front page stays intentionally uncluttered.

---

**Independent project.** ArcGIS, ArcGIS Pro, and ArcGIS Earth are Esri products and trademarks. This repository is not an Esri product or official Esri distribution.
