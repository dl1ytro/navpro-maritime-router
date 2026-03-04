# NAVPRO Maritime Route Planner

A browser-based maritime routing tool for comparing **Great Circle** and **Rhumb Line** navigation paths, generating segmented great-circle waypoints, and exporting waypoint tables in maritime coordinate format.

## Overview

NAVPRO Maritime Route Planner is a single-page web application built with plain HTML/CSS/JavaScript and rendered on an interactive Leaflet map. It helps navigators and route planners:

- Compare Great Circle and Rhumb Line distances.
- Visualize routes on a world map (including antimeridian-safe rendering).
- Split Great Circle routes into intermediate waypoints.
- Review per-leg course/distance in a waypoint table.
- Export the waypoint table to clipboard or text file.

## Key Features

- **Dual route computation**
  - Great Circle distance and curve.
  - Rhumb Line distance and line.
- **Configurable Great Circle segmentation**
  - By fixed segment count.
  - By target segment distance (nautical miles).
- **Maritime coordinate support**
  - Input format: `DD-MM.MM N DDD-MM.MM W`.
  - Decimal-to-maritime conversion for display.
- **Interactive controls**
  - Toggle visibility of Great Circle, Rhumb Line, and GC segments.
  - Quick presets for common ocean crossings.
  - Swap A/B coordinates.
  - Optional geolocation fill for Point A or Point B.
- **Waypoint table + export**
  - Shows waypoint coordinates, heading to next leg, and leg distance.
  - Copy-to-clipboard with file-download fallback.

## Project Structure

```text
.
├── index.html   # Entire application (UI + styles + route logic)
└── README.md    # Project documentation
```

## Technology Stack

- **Leaflet 1.9.4** (map rendering)
- **Tailwind CSS (CDN runtime)** (UI utility classes)
- **Vanilla JavaScript** (routing math + UI behavior)

> Note: Leaflet and Tailwind are loaded from CDN in `index.html`, so internet access is required unless vendored locally.

## How to Run Locally

Because this is a static app, you can run it with any local web server.

### Option 1: Python

```bash
python3 -m http.server 8000
```

Then open:

- `http://localhost:8000`

### Option 2: Direct file open (limited)

You can open `index.html` directly in a browser, but some browser features (especially clipboard/security-context behavior) work best over `http://localhost`.

## Usage Guide

1. Enter **Point A** and **Point B** in maritime format:
   - Example: `48-12.22 N 052-28.24 W`
2. Choose Great Circle segmentation mode:
   - **By segment count** (e.g., 3 segments), or
   - **By target segment distance** (e.g., 250 nm).
3. Toggle map layers as needed:
   - Great Circle, Rhumb Line, GC Segments.
4. Review outputs:
   - Great Circle distance,
   - Rhumb Line distance,
   - Total segmented GC leg distance,
   - Difference between Rhumb and GC.
5. Inspect generated waypoints and use **Export** to copy/save route legs.

## Coordinate Input Format

Expected format:

```text
DD-MM.MM N DDD-MM.MM W
```

Examples:

- `40-42.00 N 074-00.00 W`
- `51-30.00 N 000-07.00 W`
- `33-52.00 S 151-12.00 E`

Rules:

- Latitude uses 2-digit degrees (`DD`) and hemisphere `N`/`S`.
- Longitude uses 3-digit degrees (`DDD`) and hemisphere `E`/`W`.
- Minutes are decimal minutes with two decimals (`MM.MM`).

## Built-in Presets

The app includes quick buttons for example routes:

- NYC ↔ London
- Tokyo ↔ San Francisco
- Sydney ↔ Los Angeles
- Rio ↔ Cape Town

## Export Behavior

When exporting waypoint data:

- The app first attempts **Clipboard API** copy.
- If clipboard access is unavailable, it falls back to downloading `waypoint-table.txt`.

## Geolocation Notes

- “Use Current” uses browser geolocation.
- Browsers may prompt for permission.
- Accuracy depends on the device/browser location provider.

## Validation & Safety Notes

- Inputs are validated against a strict maritime coordinate pattern.
- Segment values are clamped/sanitized to safe minimums.
- Route polylines are handled with longitude-unwrapping and date-line split logic for cleaner world-map rendering.

## Limitations

- Uses a **spherical-earth model** for Great Circle/Rhumb calculations (not full ellipsoidal geodesy).
- CDN dependency for Leaflet/Tailwind unless replaced with local assets.
- Single-file architecture is convenient but less modular for large-scale extension.

## Suggested Future Improvements

- Split logic into modules (`/src`) and add a build step.
- Add unit tests for parsing, bearings, and distance calculations.
- Add import/export support for CSV/GPX/JSON route formats.
- Add wind/current overlays and ETA/fuel estimation tools.
- Add offline asset bundling (self-hosted Leaflet/Tailwind).

## License

No explicit license is currently defined in this repository.
If you plan to distribute or open-source this project, add a `LICENSE` file (e.g., MIT, Apache-2.0).
