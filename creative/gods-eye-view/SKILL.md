---
name: gods-eye-view
description: "Browser-based spy satellite simulator using real open-source spatial intelligence data (ICEYE, NASA, etc.)"
version: 1.0.0
author: bilawalsidhu | Weekly Discovery
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [visualization, satellite, spatial-intelligence, web-app]
---

# God's Eye View — Live Satellite Simulator

A spy satellite simulator in your browser using REAL open-source spatial intelligence data. Not a game — actual satellite imagery from public sources.

## Source

- GitHub: https://github.com/bilawalsidhu/gods-eye-view
- Stars: ~24,300 (trending)
- License: MIT
- Language: JavaScript/TypeScript

## What It Does

Renders a photorealistic 3D globe showing real satellite imagery from:
- ICEYE synthetic aperture radar (SAR)
- NASA Earth Observatory
- ESA Copernicus
- Other open-source satellite providers

Users can "fly" around the globe and see recent satellite passes over any location.

## Installation

```bash
# One-click deployment (recommended)
npx create-gods-eye-view

# Or manual install
git clone https://github.com/bilawalsidhu/gods-eye-view.git
cd gods-eye-view
npm install
npm run dev
```

## Usage

```bash
# Start local server
npm run dev

# Open http://localhost:3000
# Use mouse to rotate globe, scroll to zoom
# Click "Talk to It" for natural language queries about locations
```

## API Keys Required

Some data sources require free API keys:
- **ICEYE**: Register at https://www.iceye.com for access token
- **NASA**: No key required (public data)
- **ESA Copernicus**: Register at https://scihub.copernicus.eu

## Features

| Feature | Description |
|---------|-------------|
| Real-time orbits | Shows current satellite positions |
| Historical imagery | Access past satellite passes |
| Natural language query | Ask "show me Shanghai port activity" |
| Multi-source fusion | Combine SAR + optical data |
| Mobile support | Works on mobile browsers |

## Use Cases for Agents

1. **Location verification** — confirm infrastructure at coordinates
2. **Change detection** — compare satellite images over time
3. **Event monitoring** — track港口 activity, construction, disasters
4. **Educational demos** — show real spatial intelligence capabilities

## Technical Stack

- **Frontend**: React + Three.js + CesiumJS
- **Data**: ICEYE SAR API, NASA GIBS, ESA Sentinel
- **Deployment**: Netlify/Vercel compatible, also runs locally

## Pitfalls

- **API rate limits** — free tiers have limits; heavy usage may need paid plans
- **Image freshness** — depends on satellite pass schedule (hours to days)
- **Browser performance** — 3D rendering requires decent GPU
- **Not for classified use** — all data is open-source/public

## Verification

```bash
npm run dev
# Open browser, verify globe loads with satellite imagery
# Test "Talk to It" feature with a location query
```

## References

- GitHub: https://github.com/bilawalsidhu/gods-eye-view
- ICEYE Developer: https://developer.iceye.com
- NASA GIBS: https://gibs.earthdata.nasa.gov
