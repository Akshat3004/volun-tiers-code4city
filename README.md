# Volun-Tiers — Geospatial Volunteer Outreach Platform

**Code4City 2026 · Civic Tech · Next.js · TypeScript · Express · PostgreSQL · GIS · Gemini AI**

A full-stack, team-built platform for organizing neighborhood-based volunteer outreach in New York City. **Volun-Tiers** combines public data, OpenStreetMap points of interest, priority-aware walking routes, event coordination, and bilingual outreach materials to help volunteers plan and document their work.

> **Portfolio showcase, not a code fork:** This repository is an engineering case study maintained by [Akshat Agrawal](https://github.com/Akshat3004). The [complete original team implementation](https://github.com/DipeshK47/Code4City) remains in the collaborator-owned Code4City repository. This showcase does not claim sole authorship or include the team's original source code.

**[Explore the working source](https://github.com/DipeshK47/Code4City)** · **[Architecture](docs/ARCHITECTURE.md)** · **[Engineering details](docs/TECHNICAL_HIGHLIGHTS.md)** · **[Setup and limitations](docs/RUN_AND_LIMITATIONS.md)** · **[Credits](CONTRIBUTORS.md)**

## Why this project matters

Community resources can be hard to discover, and volunteer outreach is often planned without a unified way to identify high-need areas, avoid duplicative coverage, and track stops. Volun-Tiers explores how city open data, maps, location intelligence, and structured volunteer workflows can improve coordination.

## Project at a glance

| Dimension | Original team's documented scope |
| --- | --- |
| Geography | **197 NYC neighborhood areas**, based on NTA-level public-data coverage |
| Outreach points | **1,500+ OpenStreetMap locations** described in the team README |
| Data feeds | NYC Open Data, OSM Overpass, and additional NYC service-provider sources |
| Backend | Express 5, PostgreSQL/Supabase, JWT-authenticated API routes |
| Frontend | Next.js, React, TypeScript, maps, GPS session tracker, and organizer tools |
| AI assistance | Gemini-based meetup coordinator, optional route-ordering and translation-related workflows |
| Routes | Haversine geometry, need/coverage/time-of-day scoring, deterministic fallback |

**Evidence status:** The counts above are **team-reported dataset/ingestion figures**, not independently rerun ingestion counts. There is no verified citywide outreach impact study or production-scale traffic benchmark in this showcase.

## Architecture

~~~mermaid
flowchart TB
    N[NYC Open Data: NTA + food need] --> G[Neighborhood scoring and geometry]
    O[OSM Overpass POIs] --> H[Hotspot enrichment and priority scoring]
    G --> P[(PostgreSQL / Supabase)]
    H --> P
    P --> B[Express REST API]
    B --> R[Deterministic route planner]
    B --> A[Gemini-assisted planning and coordinator]
    R --> F[Next.js volunteer experience]
    A --> F
    B --> F
    F --> S[GPS route sessions and stop logs]
    S --> B
    F --> L[Bilingual resource flyers]
~~~

**Key design choice:** scoring neighborhoods and stops is **weighted, rule-based prioritization**, not a published supervised-learning accuracy result. Gemini is used for selected planning/coordination tasks, with deterministic fallback code where available.

## What was built

### 1. Geospatial prioritization

The backend ingests neighborhood attributes and boundaries from NYC Open Data. The documented composite-need weighting is **40% food need, 20% health access, 20% housing instability, and 20% substance-use-related indicators**, with fallbacks for missing components. OpenStreetMap hotspots are ranked using category, information completeness, and regional need context.

[Neighborhood ingestion code](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/needRegionService.js) · [OSM ingestion code](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/osmHotspotService.js)

### 2. Volunteer routes and outreach sessions

Volunteers plan visits around high-priority locations and can record GPS routes, completed stops, and activity. The route service combines travel distance, uncovered locations, coverage freshness, neighborhood need, and time-of-day suitability. The AI-assisted variant verifies model-suggested routes and can fall back to deterministic ordering.

[Route ranking logic](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/routePlannerService.js) · [AI-assisted route planner](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/aiRoutePlannerService.js) · [Session persistence](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/sessionService.js)

### 3. AI-assisted community outreach

Gemini-backed workflows help create group meetup assignments and organize outreach tasks. A flyer workflow selects relevant local service information and supports bilingual material generation. AI output is intended as **planning assistance**, not proof that resource details, translations, or routes are always correct.

[Coordinator](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/coordinatorService.js) · [Flyer service](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/flyerService.js) · [Event suggestions](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/eventSuggestionService.js)

### 4. Full-stack delivery

A Next.js user interface exposes maps, event discovery, volunteer route tracking, community meetups, flyer generation, leaderboards, and organizer monitoring. Express APIs are backed by PostgreSQL with Supabase-compatible configuration. The original project also includes a Playwright end-to-end test scenario.

[Frontend](https://github.com/DipeshK47/Code4City/tree/main/frontend) · [Backend](https://github.com/DipeshK47/Code4City/tree/main/backend) · [Playwright test](https://github.com/DipeshK47/Code4City/blob/main/frontend/e2e/events.spec.ts)

## Hackathon provenance

The [original README's hackathon transparency section](https://github.com/DipeshK47/Code4City#hackathon-transparency) distinguishes **features added during the April 26, 2026 event** (geospatial ingestion, volunteer routing, recommendations, coordinators, flyers, and testing) from **pre-existing foundations** (authentication, community feed, leaderboard, badges, messaging and basic map scaffolding). This showcase preserves that distinction.

## Run the original project

The source lives in the team repository. Set up backend credentials for Postgres/Supabase and any required Gemini/Google Maps services. See the [reproducibility guide](docs/RUN_AND_LIMITATIONS.md) before trying the application; no production deployment or independent end-to-end run is claimed here.

## Limits and responsible presentation

- **Neighborhood scores are proxies**, not validated measures of individual hardship or actual service availability.
- The route algorithm is a heuristic; **no optimality guarantee or independent 85–90% optimal-distance result** is established.
- Gemini-generated plans and translations should be reviewed before publishing or distributing outreach material.
- GPS routes, photos, and participant data require thoughtful consent and privacy controls.
- Published code and UI do not by themselves establish community adoption, impact, or runtime reliability.

## Portfolio summary

> **Volun-Tiers (Code4City 2026, team project):** Contributed to a full-stack civic outreach system integrating NYC neighborhood open data, OpenStreetMap hotspots, priority-aware route planning, Gemini-assisted coordination, and volunteer tracking; the team documented 197 NYC areas and 1,500+ outreach locations.

*This is a collaborative project. Component-level attribution should be checked against the original commit and PR history when discussing individual contributions.*
