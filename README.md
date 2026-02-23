# Kentucky Permitted Mine Boundaries Viewer

A public, read-only web application for viewing and querying mine permit boundaries in Kentucky. Built on **ArcGIS Experience Builder Developer Edition 1.19** with custom widgets.

## Purpose

Allows DMPGIS staff, field inspectors, permit applicants, and other agencies to search, filter, and inspect mine permit boundary polygons published from Portal for ArcGIS Enterprise. No authentication required.

## Data Source

**Ky Permitted Mine Boundaries (WGS84 Web Mercator)**
- Service: `https://kygisserver.ky.gov/arcgis/rest/services/WGS84WM_Services/Ky_Permitted_Mine_Boundaries_WGS84WM/MapServer/0`
- Geometry: Polygons in WKID 3857
- Key fields: `PermitNo`, `PER_NAME`, `Type_Flag`, `FeatCLS`, `Calc_Acres`, `REGION_DES`

## Custom Widgets

All widgets live in `client/your-extensions/widgets/`.

| Widget | Description |
|---|---|
| **search-filter-panel** | Permit number, operator name, status, mine type, and region filters |
| **draw-query-widget** | Draw polygon/rectangle on the map for spatial filtering |
| **results-list** | Paginated query results table (25/page) with row selection |
| **permit-detail-panel** | Full attribute detail view with map zoom and share URL |

Widgets communicate via EXB's Redux store (`appActions.widgetStatePropChange`) using a shared coordinator key.

## Tech Stack

| Component | Version |
|---|---|
| Experience Builder | 1.19 |
| ArcGIS Maps SDK for JS | 4.34 |
| React | 19 |
| Node.js | 22 |
| TypeScript | bundled with ExB |

## Getting Started

### Prerequisites

- Node.js 22
- ArcGIS Experience Builder Developer Edition 1.19 (this repo)

### Install & Run

```bash
# Install server dependencies
cd server
npm install

# Install client dependencies
cd ../client
npm install

# Start the development server
npm start
```

The ExB builder opens at `https://localhost:3001`.

### After Adding or Removing Widget Folders

Restart the client server — ExB only discovers new widget folders on startup.

## Project Structure

```
client/your-extensions/
├── CLAUDE.md                  # AI assistant context
├── manifest.json              # Extension repo manifest
├── ai_dev/                    # Design docs & field schema
│   ├── architecture.md        # Authoritative design document
│   ├── field-schema.md        # Service field names & domain values
│   ├── patterns.md            # EXB patterns & anti-patterns
│   ├── portal-endpoints.md    # Service endpoint URLs
│   └── prompt-templates.md    # Reusable development prompts
└── widgets/
    ├── search-filter-panel/
    ├── draw-query-widget/
    ├── results-list/
    └── permit-detail-panel/
```

## Import Rules (EXB-Specific)

```tsx
// React — always from jimu-core
import { React, appActions } from 'jimu-core'
const { useState, useEffect } = React

// ArcGIS JS API — always esri/ paths
import Query from 'esri/rest/support/Query'

// Settings props — always from jimu-for-builder
import type { AllWidgetSettingProps } from 'jimu-for-builder'
```

Never import React from `'react'` or ArcGIS modules from `'@arcgis/core/'` — both cause silent runtime failures in ExB's SystemJS environment.

## License

MIT
