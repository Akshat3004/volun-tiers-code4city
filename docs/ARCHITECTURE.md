# System architecture

**Original team source:** https://github.com/DipeshK47/Code4City

## Overview

The project has a **Next.js/TypeScript frontend**, **Express/Node.js backend**, and **PostgreSQL** persistence (compatible with Supabase). It consumes location and public-health/resource datasets to populate outreach rankings and route suggestions, alongside Gemini-supported coordination tools.

~~~mermaid
flowchart LR
    D[NYC Open Data] --> I[Need region ingestion]
    O[OSM Overpass] --> J[Hotspot ingestion]
    I --> DB[(PostgreSQL)]
    J --> DB
    DB --> API[Express API]
    API --> ROUTE[Heuristic / Gemini route planning]
    API --> EVENT[Events and volunteer coordination]
    API --> FLYER[Service search and bilingual flyer generation]
    ROUTE --> WEB[Next.js volunteer UI]
    EVENT --> WEB
    FLYER --> WEB
    WEB --> GPS[GPS tracker and outreach session record]
    GPS --> API
~~~

## Data layer

The **need-region** importer looks up NTA-level geometry and food insecurity data, then computes a weighted composite. Its use of proxies for missing indicators is pragmatic but means a score should not be interpreted as a measured four-factor ground truth. The hotspot importer queries OSM Overpass locations and adds heuristic category and metadata scores.

- [needRegionService.js](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/needRegionService.js)
- [osmHotspotService.js](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/osmHotspotService.js)
- [SQL schema](https://github.com/DipeshK47/Code4City/tree/main/backend/sql)

## Decision and planning layer

The deterministic planner uses haversine distance, coverage status, zone need, last proof-of-coverage age, category-by-time heuristics, and volunteer constraints. An optional Gemini-based route service proposes an ordering, with code-level validation and a deterministic fallback where model calls fail. This is **not a TSP solver** and its ranking is not a statistical learning model.

- [routePlannerService.js](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/routePlannerService.js)
- [aiRoutePlannerService.js](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/aiRoutePlannerService.js)
- [routePlannerRoutes.js](https://github.com/DipeshK47/Code4City/blob/main/backend/src/routes/routePlannerRoutes.js)

## AI coordination and outreach

A group-meetup coordinator uses neighborhood context and Gemini to generate assignments. Other services provide nearby resources, flyer context and event suggestions. These are **assistive** features requiring human review of addresses, eligibility, translations and recommendations.

- [coordinatorService.js](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/coordinatorService.js)
- [flyerService.js](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/flyerService.js)
- [eventSuggestionService.js](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/eventSuggestionService.js)

## App and validation

The Next.js frontend provides a public-facing map, volunteer event and route tools, tracker, flyers, community, and organizer views. The team repository includes a Playwright route/event flow test, but this portfolio page has **not executed that test**.

- [frontend/app](https://github.com/DipeshK47/Code4City/tree/main/frontend/app)
- [frontend/e2e/events.spec.ts](https://github.com/DipeshK47/Code4City/blob/main/frontend/e2e/events.spec.ts)
