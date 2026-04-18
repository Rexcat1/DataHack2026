# Joule

**A real-time fuel security gauge for New South Wales.**

Joule is a public-facing dashboard concept styled in the Transport for NSW livery. It gives individual travellers a live reading of how their next trip affects the state's liquid-fuel reserve — without telling them not to travel. Every choice is scored from 0 to 100, and a single actionable swap is offered when one would make a material difference.

Joule is not a substitute for the Trip Planner. It's an energy-accounting layer alongside it.

---

## Why this exists

Australia imports roughly 90% of its refined liquid fuels. As of April 2026, the national stockhold sits at **39 days of petrol and 29 days of diesel** — well below the IEA's 90-day guidance. The Strait of Hormuz closure in March–April 2026 exposed the vulnerability in real time: Sydney petrol prices peaked above 258 c/L before the federal half-excise relief and the reopening of the strait pulled them back toward 213 c/L.

Joule translates this national-scale fragility into a single, legible gauge and a trip-level score so that individual behaviour and state resilience are visible on the same page.

---

## What's in this repo

| File           | Purpose                                                         |
|----------------|-----------------------------------------------------------------|
| `index.html`   | The complete dashboard — single-file, zero build, zero backend. |
| `.nojekyll`    | Tells GitHub Pages not to run Jekyll processing on the HTML.    |
| `README.md`    | This file.                                                      |
| `LICENSE`      | MIT.                                                            |

Everything runs client-side. No dependencies beyond Google Fonts (Public Sans) loaded from CDN.

---

## Deploy to GitHub Pages

1. **Create a new GitHub repository** (public or private — Pages works with both on paid plans; public only on free).
2. **Upload the three files** (`index.html`, `.nojekyll`, `README.md`, `LICENSE`) to the repository root. The easiest way:
   ```bash
   git init
   git add index.html .nojekyll README.md LICENSE
   git commit -m "Initial Joule deployment"
   git branch -M main
   git remote add origin https://github.com/<your-user>/<your-repo>.git
   git push -u origin main
   ```
3. **Enable GitHub Pages:**
   - Go to your repo → **Settings** → **Pages**.
   - Under **Source**, select **Deploy from a branch**.
   - Under **Branch**, select **main** and folder **/ (root)**. Click **Save**.
4. **Wait 1–2 minutes.** GitHub Pages will build and publish. Your site will be live at:
   ```
   https://<your-user>.github.io/<your-repo>/
   ```

To use a custom domain, add a `CNAME` file with the domain name and configure your DNS.

---

## What the numbers are

All live figures in the dashboard are drawn from published April 2026 sources:

| Figure                              | Value                | Source                                    |
|-------------------------------------|----------------------|-------------------------------------------|
| Australia petrol stockhold          | 39 days              | DCCEEW / DISER Australian Petroleum Statistics |
| Australia diesel stockhold          | 29 days              | DCCEEW / DISER                            |
| Australia jet fuel stockhold        | 30 days              | DCCEEW / DISER                            |
| Sydney ULP average                  | 213.1 c/L            | NRMA Weekly Fuel Report, 18 Apr 2026      |
| Sydney diesel average               | 318.1 c/L            | NRMA Weekly Fuel Report, week to 14 Apr   |
| Brent crude                         | US$90.89/bbl         | ICE futures live, 18 Apr 2026             |
| Federal excise (temporarily halved) | 26.3 c/L             | Federal Cabinet decision, effective 1 Apr – 30 Jun 2026 |
| NSW VPP incentive                   | up to $1,500/hh      | NSW Peak Demand Reduction Scheme          |
| NSW peak grid demand (indicative)   | ~10.2 GW             | AEMO NEM data dashboard                   |
| VPP firming (indicative)            | 412 MW               | NSW PDRS programme reporting              |

In a production build, these are live-fetched from the respective open APIs (DCCEEW APS, AEMO NEM data, NSW FuelCheck, ACCC Fuel Price Monitoring) and refreshed every 15 minutes. In this static concept, the values are hard-coded with a small random walk on the Fuel Security Index to simulate live behaviour.

---

## How the Joule score is calculated

The score for a given trip is a weighted composite:

```
score = 0.60 × fuel_impact  +  0.25 × corridor_load  +  0.15 × time_cost
```

- **Fuel impact (60%)** — kWh-equivalent of the trip, benchmarked against a solo car on the same route at the same frequency. Active and electric transit score highest.
- **Corridor load (25%)** — how much spare capacity the recommended transit service has on the chosen corridor. A train with empty seats is worth more per walk-on than one already crowded.
- **Time cost (15%)** — a friction term that prevents the score from pretending a 3-hour coach trip is a free swap for a 90-minute drive.

Full scoring logic is in `index.html` under the `calcJouleScore` function.

---

## Corridor data

The 12 corridors in the planner are anchor points derived from the Transport for NSW commuter route dataset (April 2026 snapshot), intersected with 2021 Census journey-to-work mode share by LGA. Each corridor carries four attributes used in scoring:

- Representative transit line (`T1`, `T8`, `Metro`, etc.)
- Typical commute distance (km)
- Service frequency (trains per hour at peak)
- Peak load factor (fraction of seated capacity used)
- Car-dependence score (0–1)

For a production deployment, these would be replaced with live GTFS feed consumption so that engineering works or disruptions on a line immediately re-weight the score for trips on that corridor.

---

## Accessibility & privacy

- Built with WCAG 2.2 AA colour contrast ratios throughout.
- Public Sans typography (Transport for NSW's digital typeface).
- No tracking, cookies, or analytics. No trips are logged.
- Works fully client-side with no backend beyond CDN font loading.

---

## Licence

MIT — see `LICENSE`. Transport for NSW brand marks and NSW Government visual identity remain the property of the State of New South Wales; this repository uses the design system's published colour tokens under fair use for a non-commercial concept demonstration.

---

by Unit 8200
