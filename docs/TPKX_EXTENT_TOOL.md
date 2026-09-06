# TPKX Extent Tool v2

The TPKX Extent Tool creates and organizes **EPSG:3857 production extents**. It does not download imagery and does not create TPKX packages itself.

## Start the tool

1. Keep the supplied folder together.
2. Double-click `START_TPKX_EXTENT_PRODUCTION_TOOL.bat`.
3. Python 3 must be available on Windows.

Windows Clipboard History is needed only when using the two-GPS-point helper. You can always type or paste a HOME extent directly.

## HOME / parent extent

The HOME field is the master rectangle used by the rest of the tool. Accepted input is an optional label followed by:

```text
xmin,xmax,ymin,ymax [EPSG:3857]
```

Give the HOME extent a useful label before creating neighbors or production grids; that label flows into generated names.

## Build HOME from two GPS points

Copy two opposite-corner `lat,lon` coordinate pairs into Windows Clipboard History, then press **LOAD LAST 2 GPS POINTS FROM CLIPBOARD HISTORY**. The tool converts the two coordinates into one EPSG:3857 rectangle.

## Neighbor extents

The NW, N, NE, W, E, SW, S and SE buttons generate one same-size adjacent rectangle aligned to HOME. Use **COPY** to place the result on the clipboard.

## 24-box production split

**GENERATE 24** divides HOME into a **6 x 4** grid of twenty-four equal child extents. Use **COPY ALL** for immediate pasting into the Factory or **SAVE TXT** for an archived extent set.

Use the 24-box split when one parent is too large for practical production and you want repeatable aligned blocks.

## 10-box Z20 split

**GENERATE 10** divides one parent into a **2 x 5** grid. This is the project-standard Z20 production cut because one parent becomes one complete 10-job Factory batch.

## Hand-off to the Factory

```text
HOME extent
   -> neighbor / 24-box / 10-box output
   -> COPY ALL or SAVE TXT
   -> paste extent lines into ArcGIS TPKX Batch Factory
   -> build final TPKX files
```
