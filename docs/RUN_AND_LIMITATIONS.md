# Run the original project and understand its limitations

**The portfolio showcase is documentation-only.** To run the product, use the [original team repository](https://github.com/DipeshK47/Code4City), not this showcase.

## Development setup

Prerequisites reported by the team: Node.js, a PostgreSQL/Supabase database, and appropriate credentials for Gemini/Google Maps services where used. Confirm compatible Node versions against the package files.

~~~bash
git clone https://github.com/DipeshK47/Code4City.git
cd Code4City/backend
npm install
cp .env.example .env
# Configure DATABASE_URL, JWT_SECRET and required AI provider settings
npm run dev
~~~

Start a second shell for the frontend:

~~~bash
cd ../frontend
npm install
cp .env.example .env.local
# Configure the backend URL and relevant Maps/Gemini credentials
npm run dev
~~~

The original README documents optional importer endpoints for NYC need regions and OSM hotspots. These make network requests and can create/update database records; review them and use a disposable development database. Do not assume API keys, the external services, or demo data will be available without setup.

## Tests in the team repository

The frontend provides a Playwright end-to-end scenario:

~~~bash
cd frontend
npm run test:e2e
~~~

This command is taken from the [original README](https://github.com/DipeshK47/Code4City#testing). It has **not** been run in preparing this portfolio showcase. Tests can require a running backend, seeded database and local browser installation.

## Product and evaluation limitations

- **No independent benchmark:** the team reports 197 areas and 1,500+ points of interest, but a new import can return different counts.
- **Rule-based scoring:** the need and route scores are manually weighted; no trained-model generalization result is claimed.
- **No confirmed route optimality:** comparisons with shortest-path or TSP baselines were not independently verified.
- **Service availability:** flyers and route suggestions depend on third-party APIs; fallback behavior should be tested per endpoint.
- **Location privacy:** GPS, participant identities, meeting points and outreach photos may be sensitive; a public deployment requires authorization and data-governance reviews.
- **Accuracy and fairness:** neighborhood proxies and inferred language can mischaracterize an area; review outputs with trusted local knowledge.
- **Not a verified production service:** public code is not evidence of uptime, real users or measured outreach impact.
