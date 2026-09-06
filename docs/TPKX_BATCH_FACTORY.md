# ArcGIS TPKX Batch Factory

The Batch Factory is an ArcGIS Pro add-in that turns controlled **EPSG:3857 extents** into offline TPKX packages using ArcGIS Pro's native tile-cache export path.

## What the operator supplies

- Map product: **Satellite Imagery**, **Road Overlay**, or **Both**.
- Maximum zoom: **Z16, Z17, Z18, Z19, or Z20**.
- Destination folder.
- **1-10** canonical EPSG:3857 extent lines.

The visible map in ArcGIS Pro is not the production map. The Factory creates private maps for preflight and production, keeping each job isolated.

## Before BUILD BATCH

1. Open ArcGIS Pro and launch the Factory add-in.
2. Keep internet access available during production; the Factory is building offline files but must retrieve the selected tiles while it runs.
3. Choose a destination with enough free space.
4. Paste 1-10 extent lines.
5. Choose the product and maximum zoom.
6. **Leave the destination folder alone once the batch starts.**

Do not rename, move, copy, add, or delete TPKX files in the destination during a run. The safety logic compares filenames before and after each job.

## Running a batch

1. Select the map product.
2. Select the maximum zoom.
3. Select the destination.
4. Paste the extents.
5. Press **BUILD BATCH**.
6. Watch the Job and Status indicators.

The Factory preflights all requested extents before starting production. It does not silently lower an unsupported zoom.

## Between jobs

Each extent is exported from a newly created private map. After every third completed job, if more jobs remain, the Factory waits **120 seconds** before starting the next group.

## Safe cancellation

**CANCEL BATCH** allows the current ArcGIS export to finish safely and then prevents the remaining jobs from starting.

## Outputs and protection

Typical output names:

```text
World_Imagery Grid <LABEL>.tpkx
Reference_World_Transportation Grid <LABEL>.tpkx
```

If an expected output already exists, the batch does not start. Each run also writes a timestamped batch log to the destination folder.

For a single-product job the Factory expects exactly one new TPKX file; for **Both**, exactly two. A different count triggers a safety stop.

## After the batch

1. Confirm all intended jobs show PASS.
2. Verify the output files and names.
3. Rename only after the Factory is completely finished.
4. Archive the finished maps.
5. Experiment only on copies, not the archive originals.
