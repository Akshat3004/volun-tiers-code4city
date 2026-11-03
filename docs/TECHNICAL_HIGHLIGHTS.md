# Technical highlights and evidence

These are implementation details and scope descriptions supported by the [original repository](https://github.com/DipeshK47/Code4City). They are **not independently rerun results**.

| Feature | What can be shown | Evidence |
| --- | --- | --- |
| Open data | NYC neighborhood-need scores and NTA geometry | [Importer](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/needRegionService.js) |
| Hotspot data | OSM Overpass POI ingestion with source metadata, type and priority | [Importer](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/osmHotspotService.js) |
| Route planning | Deterministic prioritization over distance, need, coverage and time windows | [Planner](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/routePlannerService.js) |
| AI route help | Gemini route-order proposals with validation and fallback | [AI planner](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/aiRoutePlannerService.js) |
| Meetups | Gemini-backed volunteer assignment planning | [Coordinator](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/coordinatorService.js) |
| Localization | Nearby resource lookup and bilingual flyer workflow | [Flyer service](https://github.com/DipeshK47/Code4City/blob/main/backend/src/services/flyerService.js) |
| Data persistence | PostgreSQL schema and route/session tables | [SQL](https://github.com/DipeshK47/Code4City/tree/main/backend/sql) |
| Frontend workflow | Maps, sessions, events, messages, community and admin UI | [Next.js app](https://github.com/DipeshK47/Code4City/tree/main/frontend/app) |
| Regression scenario | Playwright event and stop-completion E2E test exists | [Test](https://github.com/DipeshK47/Code4City/blob/main/frontend/e2e/events.spec.ts) |

## Team-documented scale

- **197 NYC neighborhood areas** described as part of the imported NTA coverage.
- **1,500+ hotspots/POIs** described in the original project README.
- **13 geographic Overpass query subregions**, **20 POI category filters** according to the team documentation.

The numbers refer to the team's dataset import/demo scope, **not users served, volunteers onboarded, or independently audited production counts**.

## Composite need formula

The original code specifies the following weights:

~~~text
need = 0.40 × food_need
     + 0.20 × health_access
     + 0.20 × housing_instability
     + 0.20 × substance_use_indicators
~~~

Because some data fields can be missing and filled with proxies, this composite is an **engineered heuristic**. It should not be marketed as a learned predictor or certified measure of inequality.

## Engineering decisions worth discussing in interviews

1. **Combining inconsistent public-data schemas:** normalize geography, coordinates, identifiers, and sparse need fields before using them in ranking.
2. **Bounded AI decisions:** use structured candidate stops and validate model output before applying a route; recover from provider failures with deterministic planning.
3. **Spatial trade-offs:** priority and route distance conflict; keep both interpretable instead of claiming a globally shortest route.
4. **Frontend-to-backend flow:** volunteer session state connects itinerary planning, location tracking, coverage and organizer summaries.
5. **Geolocation and privacy:** mobile GPS and community activity data need explicit user consent, access controls and retention decisions before deployment.

## Caveats about results

- Route optimality percentages in the original README are not backed by independently inspected comparative experiment reports; do not claim them as benchmarks.
- No quantitative model evaluation metrics or verified impact study were recovered for the AI coordinator, language translation or real-world outreach quality.
- Presence of a Playwright test file does not establish that it passed in this conversation.
