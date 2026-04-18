# Petal — a date route planner

A small web app that takes a handful of places you want to visit in one day and works out the fastest order to see them all, then hands the finished route off to Google Maps for turn-by-turn directions.

Built to solve a real problem: my girlfriend and I would spend ten or fifteen minutes manually rearranging stops in Google Maps before every multi-stop date. This does it in a second.

**Live at:** [kensent.github.io/petal-the-date-planner](https://kensent.github.io/petal-the-date-planner/)

## How it works

1. Add every place you want to visit, in any order.
2. Choose your starting point (the first stop you added, or your current location).
3. Tap *Find the best route*. The app computes travel times between every pair of stops and solves for the shortest total route, then shows the reordered itinerary with a one-tap *Open in Google Maps* button.

Under the hood: the Traveling Salesman Problem is solved optimally via Held-Karp dynamic programming for up to 12 stops, with a nearest-neighbor + 2-opt heuristic as a fallback for larger inputs.

## Stack

Single `index.html`, no build step, no backend. Plain HTML/CSS/JS.

- **Geocoding & place search:** [Photon](https://photon.komoot.io/) (primary) and [Nominatim](https://nominatim.org/) (fallback), both powered by OpenStreetMap data. Optional upgrade to [Google Places](https://developers.google.com/maps/documentation/places/web-service) if the user provides their own API key.
- **Travel-time matrix:** [OSRM](https://project-osrm.org/) public demo server for driving. Walking and cycling fall back to haversine distance with a speed assumption.
- **Navigation hand-off:** [Google Maps URLs API](https://developers.google.com/maps/documentation/urls/get-started) — no key required for this part.
- **Location bias:** browser timezone for country-level filtering; optional precise coordinates if the user grants geolocation.

## Running it yourself

```bash
# Clone and serve
git clone https://github.com/KensenT/petal-the-date-planner.git
cd petal-the-date-planner
python3 -m http.server  # or any static server
# Open http://localhost:8000
```

Or just drop `index.html` on GitHub Pages, Netlify, Cloudflare Pages, or any static host. There's no build step.

### Optional: Google Places API key

The default setup uses OpenStreetMap data, which covers most places but has gaps (small businesses in certain regions, for example). Plugging in a Google Maps API key unlocks richer place search and exact pin accuracy when the route opens in Google Maps.

To set one up: open Settings in the app and follow the guide. The key is stored only in your browser's localStorage and is restricted by HTTP referrer to your domain, so it can't be used from anywhere else. A daily quota cap in the Google Cloud Console is a good idea as an extra safety net.

## License

[MIT](LICENSE). Do what you like with it.

## A note on authorship

This is an honest collaboration: product direction, testing, and most of the debugging decisions are mine. The code itself was written iteratively with [Claude](https://claude.ai) as a pair programmer over many turns. Neither of us could have shipped it alone — Claude wouldn't have known what problem to solve or when its output was wrong; I wouldn't have written 3,000 lines of TSP solvers, Google Places integrations, and flex-layout edge-case handling in the same evening.

If you're curious how that process actually looked — including all the dead ends, course corrections, and "that's not what I meant" moments — the commit history tells the story.