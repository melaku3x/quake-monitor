# Quake Exposure Monitor

**Live site: https://melaku3x.github.io/quake-monitor/**

A real-time map of every M4.5+ earthquake from the past week, with a quick check on how many people live close enough to feel it.

Most earthquake maps show you where a quake hit. I wanted mine to answer the next question too: who's actually affected? A M6.9 in the middle of the ocean and a M6.0 next to a city of a million people aren't the same story, and the map should make that obvious.

## What it does

- **Live earthquake feed.** Pulls the latest M4.5+ events from the USGS every time the page loads.
- **Interactive globe.** Drag to spin it, click any quake to zoom in. Dot size shows magnitude and color shows depth (shallow quakes are red, since those usually hit the surface hardest).
- **Exposure check.** For each quake, it draws rings at 50, 100 and 250 km and counts the cities and people inside them.
- **Concern label.** Each event gets a simple Low / Watch / High label based on magnitude, depth and how many people are nearby.
- **Replay mode.** Scrub or play back the whole week to see how activity built up over time.

## How the exposure analysis works

This is the GIS part, and it's the same workflow I'd do in ArcGIS Pro, just automated:

1. **Buffer.** Draw circles at 50, 100 and 250 km around the epicenter.
2. **Spatial join.** Check which cities fall inside each circle.
3. **Summarize.** Add up the population in each ring.

Distances are measured along the curve of the Earth (haversine formula), not as straight lines on a flat map, since a flat map stretches distances a lot near the poles.

The concern label follows one rule: strong, shallow quakes close to big populations rank highest. It's a screening tool to flag what's worth a closer look, not a damage forecast.

## Data

- **Earthquakes:** [USGS Earthquake Hazards Program](https://earthquake.usgs.gov/earthquakes/feed/) real-time GeoJSON feed
- **Cities and population:** [Natural Earth](https://www.naturalearthdata.com/) populated places, every city over 20,000 people
- **Country borders:** Natural Earth, through the world-atlas package

## Built with

- **D3.js** for the globe, projection and timeline
- **TopoJSON** for the country borders
- Plain HTML, CSS and JavaScript, hosted on GitHub Pages

I built this with help from Claude (Anthropic's AI) for the code, while I focused on the analysis design and what the map should show.

## Limitations

- Population counts only include cities over 20,000 people, so rural areas get undercounted.
- The rings are simple distance buffers. Real shaking depends on fault type, local geology and building quality, which this doesn't model yet.

## What's next

- Swap the city list for a gridded population dataset (WorldPop or GHSL) so every person gets counted, not just people in cities
- Add tropical storms and volcanic activity
- Flag nearby infrastructure like airports, ports and power plants
- Alerts for big quakes near large populations

Every hazard map can tell you where. The useful ones tell you who.
