[README.md](https://github.com/user-attachments/files/34887286/README.md)
# Mars Survival Guide — *Martians Are Not At Home*

A web app about Jezero crater, Mars, in two halves that share the same data:

- **Story mode — Jezero Control Post.** You are Operator 6767 at a remote driving post. Power up the office, take an expedition, and drive a rover across the real crater while four colleagues call you on the UHF radio.
- **Science mode — the Jezero map.** The crater in 3D and 2D with the measurements behind the game: elevation, thermal inertia, geology, minerals, weather, radiation and the moons. Every layer carries its source.

The whole thing is three static HTML files. No build step, no server, no dependencies to install.

---

## Running it

Open `index.html` in a browser. That is all.

For the live moon positions you need the one redirect in `_redirects`, so the app is easiest to host on **Netlify**: drag the folder onto the Netlify dashboard, or connect the repository and leave the build command empty with the publish directory set to the repository root.

```
/api/horizons  https://ssd.jpl.nasa.gov/api/horizons.api  200
```

The browser cannot call the JPL Horizons API directly (no CORS header), so Netlify forwards the query. **On GitHub Pages or any host without that rule the app still works**: it says so on screen and falls back to a computed two-body ephemeris for Phobos and Deimos, labelled as simulated.

Requirements: a browser with WebGL2. `three.js` r128 and `Leaflet` 1.9.4 load from a CDN; everything else — terrain, imagery, measurements — is embedded in the files.

---

## Story mode

**Driving.** `W` / `S` drive, `A` / `D` turn, drag to orbit the camera, wheel to zoom, `P` for the autopilot. On a touch screen there is a pad on the navigation monitor.

**Before each run**, in the 3D office: turn the power on at the fuse cage, check in at the terminal by the door, then pick an expedition at the main computer. Upgrades and snacks are optional.

**The crater is continuous.** The drivable ground is the whole mapped crater — about 88 × 65 km — at 1:1, not a small arena. Terrain height comes from the elevation model, soil type from thermal inertia and the geologic map, and the expeditions are placed by searching the data for the ground each one actually needs: clay-bearing flats for greenhouse soil, dune fields for a drift survey, a carbonate outcrop for the delta sample, a local high point for the relay.

**The radio.** The call sheet is a UHF transceiver. Each contact holds one channel and you only hear the one the dial is on; when somebody calls on another frequency the set flashes, the signal meter rises as you sweep towards them, and `AUTO-SCAN` finds them for you.

| Channel | Who | What for |
|---|---|---|
| 401.55 | Alan Korwel, science sub-director | objectives, bearings, what the orbital data says about the ground under you |
| 404.20 | M.-R. Turner, managing director | mission reports, citations, bonuses |
| 437.10 | George McKay, maintenance engineer | traction, extraction, chassis repairs |
| 415.80 | Victor Rockwell, external client | buys landscape photographs |

Each voice is synthesised separately — pitch, waveform, pace and ring cadence per character.

**When the wheels turn and the rover does not move** — high-centred on a rock, or bogged in a hollow — the post says why. `UNSTICK` (`R`) rocks it back onto clear ground for free; if there is nowhere to roll to, George sends Ingenuity to lift it out, for credits and integrity.

**CAM-01.** `CAPTURE` (`C`) stores a still. Its quality is scored from the same data as everything else: relief in frame, sun elevation, dust opacity, rover tilt, the mineral under the camera, the rarity of the geologic unit and how far you are from the lander.

---

## Science mode

The crater in 3D (elevation-driven mesh, orbital imagery draped over it) and in 2D (Leaflet, metres, `L.CRS.Simple`), with hillshade, 100 m contours and a slope layer. Hover anywhere to read coordinates, elevation, thermal inertia, geologic unit and dominant mineral. Plan a route and the planner scores the candidates — safest, balanced, fastest — against slope and ground type, then an autopilot drives the chosen line across the 3D terrain.

Routes planned here can be sent to the game as an expedition.

---

## Where the data comes from

The map grid is 1020 × 756 px covering 77.00680°–78.57920° E and 17.89008°–18.99956° N — about 86.8 × 87.1 m per pixel.

| Layer | Source |
|---|---|
| Elevation | `M20_JezeroCrater_CTXDEM_20m.tif` — USGS / JPL Mars 2020 CTX DEM mosaic, 20 m/px, heights relative to the MOLA datum. Resampled to the map grid; 99.3% coverage; −3329 m to −486 m. |
| Thermal inertia | Edwards, C. S., & Buz, J. (2021). *THEMIS-derived Thermal Inertia of Jezero Crater, Mars* (Version 1) [Dataset]. Zenodo. https://doi.org/10.5281/zenodo.4931348 — CC BY 4.0. THEMIS / 2001 Mars Odyssey, 100 m/px. 66% coverage; differences under about 20 units are within the uncertainty. |
| Geology | Sun, V. Z., & Stack, K. M. (2020). *Geologic map of Jezero crater and the Nili Planum region, Mars.* USGS Scientific Investigations Map 3464, 1:75,000. https://doi.org/10.3133/sim3464 — mapped from orbit before Perseverance landed; covers the western half of this map. |
| Minerals | Parente, M., Itoh, Y., & Saranathan, A. M. (2021). *Map of the dominant mineralogy over and around the Mars 2020 landing ellipse.* Zenodo. https://doi.org/10.5281/zenodo.5575824 — from CRISM scenes HRL000040FF, FRT000047A3, FRT00005C5E, FRS00031442. Context from Horgan et al. (2020). |
| Weather | Model built on published MEDA measurements from Perseverance: Rodríguez-Manfredi et al. 2023 (*Nature Geoscience*); Munguira et al. 2023; Viúdez-Moreiras et al. 2022; Sánchez-Lavega et al. 2023; Lemmon et al. 2022; Hueso et al. 2023 (*JGR Planets*, *GRL*). Clock and season from the NASA PDS MEDA catalogue. |
| Radiation | Quiet rate 0.64 mSv/day, measured by RAD on Curiosity in Gale crater: Hassler et al. 2014, *Science* 343. Solar particle events: Zeitlin et al. 2018, *GRL* 45. |
| Moons | NASA/JPL Horizons, queried live through the redirect above. |
| Imagery | NASA/JPL-Caltech orbital imagery of Jezero. |

---

## What is real and what is not

The project's one rule is that you can always tell. On screen, every number says where it came from; here is the short version.

**Real:** the terrain you drive on and every elevation, slope and coordinate derived from it. Thermal inertia, geologic units and dominant minerals, at the resolution of their sources. The weather model's parameters and the seasonal clock. The quiet radiation level. The moon ephemerides, when the redirect is in place.

**Simulated, and labelled as such on screen:**

- **Radiation alerts.** Perseverance carries no radiation detector, so Jezero has no measurement. The quiet level is Curiosity's, from another crater; the alert-level events are dramatised — the strongest solar event measured on the surface raised the dose rate two to three times, not more.
- **Weather in the moment.** A model fitted to published MEDA statistics, not a forecast or a measurement of any particular sol.
- **The radio.** The ~400 MHz UHF relay band is the real one Perseverance uses to reach the orbiters (Electra-Lite radio, NASA/JPL, L3Harris). The individual channel numbers, the post, the agency and the four colleagues are invented for the game.
- **Everything about the rover as a vehicle**: slip, sinkage, damage, energy, the time limits, the credits, the citations, the minigames. Game design, not engineering data.
- **Anything smaller than one map pixel** (~87 m): individual rocks, dunes, ripples and the soil texture are generated from the data of the cell they sit in, not observed.

---

## Layout

```
index.html    main menu — pick story mode or science mode
rover.html    story mode: 3D office, navigation monitor, rover, radio  (~9 MB)
map.html      science mode: 3D and 2D Jezero map with all layers       (~7 MB)
_redirects    Netlify proxy for the JPL Horizons API
```

Each page is self-contained: the elevation model, thermal inertia, geology, mineral map and imagery are embedded as base64 PNG/JPEG, which is why the files are large. They are meant to be served as they are, not bundled.

---

## AI assistance

Parts of this project were built with AI assistance (Anthropic Claude), declared here, in the `ai-assistance`
metadata of each HTML file, and on the project submission page.

**Written by the team, without AI:** the concept and the approach to the challenge, the four characters and
every line of their dialogue, the portrait art, and the direction of every build.

**Built with AI assistance:** the data pipeline that turns the CTX DEM, THEMIS, USGS and CRISM products into
the embedded layers; the 3D terrain, the continuous crater and the science map; the route planner, autopilot
and navigation; the radio, the traction-recovery and the camera systems; the interface and the test harness.
The characters' dialogue was adapted from the team's script into the branching trees the game uses, but the
words are the team's.

No AI-generated imagery is used anywhere in the project, and no generated content contains NASA branding,
logos or mission identifiers. Mission names appear only as attribution for the data.

## Credits

Built as a hackathon project. Data belongs to the missions and authors listed above; the THEMIS thermal inertia dataset is CC BY 4.0 and the USGS and NASA material is public domain. Add a `LICENSE` file for the code before sharing the repository.
