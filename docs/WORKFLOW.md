# End-to-End Offline TPKX Workflow

This repository is organized around a simple production chain rather than a collection of experiments.

## 1. Define the map area

Use the **TPKX Extent Tool** to create or load a parent EPSG:3857 extent. From there you can generate aligned neighboring blocks, a 24-box 6 x 4 production split, or the project-standard 10-box 2 x 5 split used for Z20 production.

## 2. Build the TPKX packages

Paste the resulting extent lines into the **ArcGIS TPKX Batch Factory** inside ArcGIS Pro. Select Satellite Imagery, Road Overlay, or Both; choose Z16-Z20; select a destination; and run the batch. The Factory preflights all requested extents before production.

**Important:** the Factory creates offline packages, but the production computer needs internet access while it downloads tiles from the selected Esri source. Do not edit the destination folder while a batch is running.

## 3. Verify, archive, and deploy

Verify the completed TPKX files, archive them, and deploy copies to the field hardware. This project has exercised TPKX archives on Windows PCs/laptops, Android phones/tablets, and an Amazon Fire tablet.

## 4. Mobile offline use

ArcGIS Earth Mobile supports local TPK/TPKX files. In project testing, an Android phone opened TPKX files from internal storage, microSD, and exFAT USB storage. Removable-storage behavior can vary by Android file picker/OEM; opening the file through Android's file manager has been more reliable than ArcGIS Earth's in-app picker on some devices.

## 5. Optional live field tracking

The **PRAVE / ME Tracker** is independent of the TPKX production chain. It reads mixed RMC/PRAVE serial data and uses ArcGIS Earth's local Automation API for drawings and camera control. The operator can keep the map at a useful incident scale while ME remains centered.
