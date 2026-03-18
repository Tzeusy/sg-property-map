# Tze's HouseHunt

Interactive Singapore property market explorer with transaction heatmaps, geo overlays, and accessibility scoring.

<p align="center">
  <a href="https://tze.how/sg-property-map/">
    <img src="https://img.shields.io/badge/Live_Site-tze.how-0ea5e9?style=for-the-badge" alt="Live Site" />
  </a>
  <a href="https://tze.how/blog/property-hunt">
    <img src="https://img.shields.io/badge/Blog_Post-tze.how-f97316?style=for-the-badge" alt="Blog Post" />
  </a>
</p>

## What is this?

A static, read-only property map for Singapore that visualizes:

- **Transaction heatmaps** &mdash; URA resale/rental transactions colored by PSF (price per square foot), filterable by date range, property class, room count, and size
- **Geo overlays** &mdash; MRT lines, hawker centres, schools, parks, supermarkets, clinics, sports facilities, and more
- **Accessibility scoring** &mdash; composite walkability/livability heatmap based on proximity to amenities, weighted by configurable profiles (family, commuter, foodie, etc.)
- **Residential listings** &mdash; current rental and sale listings with PSF labels

No login required. All data is bundled at build time.

## Architecture

This repository contains a **pre-built static bundle** &mdash; HTML, JS, CSS, and data files &mdash; published automatically from a larger property research system. It is deployed via GitHub Pages.

```
┌─────────────────────────────────────────────────────────────┐
│                    Data Pipeline (private)                   │
│                                                             │
│  ┌──────────┐   ┌──────────┐   ┌──────────┐               │
│  │ URA API  │   │ Property │   │ OneMap /  │               │
│  │ (txns)   │   │ Portals  │   │ Geo APIs │               │
│  └────┬─────┘   └────┬─────┘   └────┬─────┘               │
│       │              │              │                       │
│       └──────────────┴──────────────┘                       │
│                      │                                      │
│              ┌───────▼────────┐                             │
│              │   PostgreSQL   │                             │
│              │ PostGIS + vec  │                             │
│              └───────┬────────┘                             │
│                      │                                      │
│              ┌───────▼────────┐                             │
│              │  Static Export │                             │
│              │  (17 datasets) │                             │
│              └───────┬────────┘                             │
│                      │                                      │
│              ┌───────▼────────┐                             │
│              │  Vite Build    │                             │
│              │  (React PWA)   │                             │
│              └───────┬────────┘                             │
└──────────────────────┼──────────────────────────────────────┘
                       │  git push
              ┌────────▼────────┐
              │  This Repo      │
              │  (GitHub Pages) │──── tze.how/sg-property-map/
              └─────────────────┘
```

### Bundled Datasets

| Category | Datasets |
|----------|----------|
| **Heatmaps** | Transaction heatmap (combined, sale, rental), price heatmap (sale, rental), accessibility heatmap (sale, rental), filterable heatmaps, bootstrap index |
| **Geo overlays** | Amenities (hawker centres, schools, parks, clinics, etc.), transit overlay (MRT/LRT lines + stations), transport styling |
| **Listings** | Residential listings (rental + sale with PSF) |
| **Metadata** | Accessibility signal weights, geodata freshness timestamps |

## Tech Stack

- **React** + **TypeScript** (Vite)
- **Leaflet** for map rendering
- **Canvas heatmaps** with WebGL-accelerated tile rendering
- Fully client-side &mdash; no backend API calls at runtime

## Development

This is an auto-published output repository. To make changes, contribute to the source project and re-publish.

## License

All rights reserved.
