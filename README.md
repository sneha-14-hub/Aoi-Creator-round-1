# AOI Creator — Frontend Engineer Internship Assignment

**Stack:** React, TypeScript, Vite, Tailwind CSS, Playwright, react-leaflet (Leaflet)

## Demo
- Run locally: `npm install && npm run dev`
- Default dev URL: `http://localhost:5173`

## Project Structure
(brief structure as shown earlier)

## Setup
1. `git clone <repo>`
2. `cd frontend-aoi`
3. `npm install`
4. `npm run dev`

## Map library choice
**Chosen:** Leaflet via `react-leaflet`.

**Why:** 
- Native simplicity for raster WMS layers via `L.tileLayer.wms`.
- Lightweight and well-documented.
- Mature plugin ecosystem (draw, clustering) that helps meet bonus requirements quickly.

**Alternatives considered:**
- **MapLibre GL** — better for high-performance vector rendering and styling; larger setup when mixing raster WMS + vector but better if target is thousands of dynamic vector features.
- **OpenLayers** — very powerful and granular WMS support; steeper learning curve and larger API surface.

## Architecture decisions
- UI: React components (Sidebar, MapView) with Tailwind for styling matching Figma.
- Map needs and drawing are encapsulated in `MapView`.
- State: React Context + hooks for AOI management; persisted to `localStorage`.
- Tests: Playwright for E2E, tests in `tests/playwright`.

## Performance considerations (1000s of points/polygons)
- Render vector features as GeoJSON layers, not individual DOM elements.
- Use **marker clustering** (leaflet.markercluster) or vector tiling for many markers.
- Use Web Workers for heavy geometry operations (simplification, intersection).
- Use lazy loading / viewport-based rendering (only render features within extent).
- Use debouncing for expensive map events (e.g., on moveend).

## Testing strategy
- **Playwright E2E**: map loads, WMS tiles requested, draw/persist functionality.
- **Unit tests** (recommended): utilities (storage, geojson helpers) and small components.
- **What I'd test given more time**: geometry simplification functions, offline fallback, accessibility keyboard interactions.

## Tradeoffs made
- Chose Leaflet for speed of prototyping & WMS ease; MapLibre/OpenLayers might be better for production at scale.
- Persistence via `localStorage` (simple) vs backend persistence (robust) — for assignment, localStorage meets requirements and demonstrates persistence.

## Production readiness (what I'd add)
- Backend to persist AOIs per-user (API + DB).
- Authentication & authorization.
- ESLint + Prettier + CI pipeline, Playwright test runner in CI.
- Accessibility improvements (ARIA on controls).
- S3 for large tile caching or tile server proxy if necessary.

 ## API endpoints (examples)

- GET /api/aoi → list AOIs for user
- POST /api/aoi → create new AOI (body: geojson)
- GET /api/aoi/:id → get AOI
- PUT /api/aoi/:id → update AOI
- DELETE /api/aoi/:id → delete AOI

## Time spent
- Rough estimate: (fill actual times when you implement)
  - Setup & skeleton: 2–3 hours
  - Map integration + WMS: 2 hours
  - Draw tools + persistence: 3 hours
  - Playwright tests: 1.5 hours
  - README + demo video prep: 1 hour

## How to run tests
1. `npx playwright install`
2. `npm run test:e2e`

