# End-to-End Offline TPKX Workflow

This repository is organized around a simple production chain rather than a collection of experiments.

## 1. Define the map area

Use the **[Master 4 Grid Maker](https://github.com/Jim-dc95811/QGIS-Mbtile-to-ArcGIS-Earth-TPKX/blob/main/MASTER4_GRID_MAKER.md)** to split a 1-degree Master 4 box into 100 numbered cells. Its TXT file lists each cell's EPSG:3857 extent, and its numbered GeoTIFF shows the cells over the map. The same cell numbers are used by Parcel100 and the other Master 4 tools.

## 2. Build the TPKX packages

Paste up to 10 cell rows at a time into the **ArcGIS TPKX Batch Factory** inside ArcGIS Pro. Delete the file name at the end of each row first: the Factory accepts `01 xmin,xmax,ymin,ymax [EPSG:3857]` but rejects the row with the `.tpkx` name still on the end. Select Satellite Imagery, Road Overlay, or Both; choose Z16-Z20; select a destination; and run the batch. The Factory preflights all requested extents before production.

**Important:** the Factory creates offline packages, but the production computer needs internet access while it downloads tiles from the selected Esri source. Do not edit the destination folder while a batch is running.

## 3. Verify, archive, and deploy

Verify the completed TPKX files, archive them, and deploy copies to the field hardware. This project has exercised TPKX archives on Windows PCs/laptops, Android phones/tablets, and an Amazon Fire tablet.

## 4. Mobile offline use

ArcGIS Earth Mobile supports local TPK/TPKX files. In project testing, an Android phone opened TPKX files from internal storage, microSD, and exFAT USB storage. Removable-storage behavior can vary by Android file picker/OEM; opening the file through Android's file manager has been more reliable than ArcGIS Earth's in-app picker on some devices.

## 5. Optional live field tracking

The **PRAVE / ME Tracker** is independent of the TPKX production chain. It reads mixed RMC/PRAVE serial data and uses ArcGIS Earth's local Automation API for drawings and camera control. The operator can keep the map at a useful incident scale while ME remains centered.
