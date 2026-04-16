# C4ST — Coastal Forecast Tool

A comprehensive coastal forecast app for Australian surfers, paddleboarders, and ocean enthusiasts. Dark UI, data-dense, combines tide/swell/wind into a single unified graph with custom notifications. Built as a React/Vite web app, wrapped with Capacitor for iOS.

---

## 🚀 App Store Submission Status

**App Store ID:** 6761449929 | **Bundle ID:** `com.fourcast.app` | **Version:** 1.2 / **Build 10** | **Status:** ✅ READY FOR SUBMISSION

| Item | Status | Notes |
|---|---|---|
| Custom Alerts Feature | ✅ Done | Dual-range sliders, compass arc pickers, rule matching engine |
| GeoJSON Coverage File | ✅ Done | 6 Australian coastal states, included in bundle |
| Encryption Compliance | ✅ Done | `ITSAppUsesNonExemptEncryption: false` in Info.plist |
| Privacy policy URL | ✅ Done | `https://raw.githubusercontent.com/smoled/coastcast/main/PRIVACY.md` |
| Support Website | ✅ Done | https://c4st-website.vercel.app (FAQ + contact form) |
| App icon (1024×1024) | ✅ Done | Full-bleed, no pre-rounded corners |
| iPhone screenshots | ✅ Uploaded | |
| iPad 13-inch screenshots | ✅ Available | 3 PNGs on Desktop, ready to upload |
| Category | ✅ Weather | |
| Promotional text | ✅ Ready | "Tides, swell, wind — all in one forecast..." |
| Content Rights | ⚠️ Declare in App Store Connect → App Information |
| App Privacy questionnaire | ⚠️ Complete in App Store Connect → App Privacy |
| Archive & Upload | ⏳ Next Step | Product → Archive, then upload to App Store Connect |

### Promotional Text (ready to paste)
```
Tides, swell, wind — all in one forecast. The app built by surfers for everyone who reads the ocean.

C4ST provides unified forecasts for 73 Australian beaches with custom alerts, smart scoring, and real-time wind maps. Set your perfect conditions and get notified days ahead when the forecast lines up.
```

### GitHub & Deployment
```
App Repo: https://github.com/smoled/coastcast
Website Repo: https://github.com/smoled/c4st-website
Live Website: https://c4st-website.vercel.app
Support Page: https://c4st-website.vercel.app/support
```

---

---

## 📱 Key Features (v1.2)

### 1. Custom Notifications (NEW)
- Per-beach alert rules with dual-handle range sliders
- Swell height, period, wind speed ranges
- Wind & swell direction pickers (compass arc, 360° selection)
- Tide preference (any/low/mid/high)
- Lead time (1-7 days advance notice)
- Same-day notification toggle
- Notifications fire when 7-day forecast matches all conditions

### 2. Smart Scoring
- Evaluates every 3-hour forecast window
- Rates conditions from Poor → Great
- Finds optimal "best time to surf" session
- Considers swell, wind, tide, daylight

### 3. Unified Forecast Graph
- Tide, swell, wind on single SVG chart
- Hour-by-hour data across 7 days
- Water temperature & UV index
- Sunrise/sunset times

### 4. Wind Map
- Real-time particle visualization (Leaflet + canvas)
- Interactive beach markers (60 beaches within 250km)
- Wind speed legend + timeline scrubber
- Responsive to layout changes

### 5. Beach Coverage
- 73 beaches across 6 Australian coastal states
- Browse by state or search by name
- Save favorites
- Nearby spots finder (geolocation)

## Quick start

```bash
cd "/Users/sol/Documents/Claude/Tidal APP/coastcast"
export PATH="/opt/homebrew/bin:$PATH"   # node lives in Homebrew
npx vite --host                          # dev server on :5173
npm run build && npx cap sync ios        # build + sync to iOS
open ios/App/App.xcodeproj               # open Xcode for archive/upload
```

---

## Tech stack

- **React 19 + Vite 8** (vanilla template, no router, no state lib)
- **Capacitor 8.3** for iOS wrapper
- **Capacitor plugins**: `@capacitor/geolocation`, `@capacitor/share`, `@capacitor/filesystem`, `@capacitor/local-notifications`, `@capacitor/splash-screen`, `@capacitor/status-bar`
- **Open-Meteo API** — weather (`api.open-meteo.com`) + marine (`marine-api.open-meteo.com`)
- **Leaflet + custom canvas particle layer** — wind map visualization
- **Fonts**: Outfit (headings) + JetBrains Mono (data) loaded from Google Fonts in `index.html`
- **Styling**: 100% inline styles, no CSS framework
- **No chart libraries** — all SVG hand-drawn in `Graph.jsx`
- **Bundle ID**: `com.fourcast.app` | **iOS deployment target**: 15

---

## Project structure

```
coastcast/
├── src/
│   ├── App.jsx                      Main component: home / detail views, settings, nav
│   │                                 + alertModalOpen state, bell icon, CustomAlertModal integration
│   ├── main.jsx                     Entry; global wheel listener with nested scroll detection
│   ├── theme.js                     THEMES.dark / .light color system
│   ├── index.css                    iOS scroll lock + input zoom prevention
│   ├── data/
│   │   ├── beaches.js               73 beaches across 6 AU states + STATES array
│   │   └── sharks.js                Mock shark sighting data + getSharkAlertLevel()
│   ├── components/
│   │   ├── Graph.jsx                Unified SVG chart (tide/swell/wind layered)
│   │   ├── DayDetail.jsx            Zoomed day card (overview, tides, swell, wind, conditions, compass, sharks)
│   │   ├── WindMap.jsx              Full-screen wind map (Leaflet + canvas particles + beach markers)
│   │   │                              + resizeFnRef, useEffect on [searchOpen] for iOS resize fix
│   │   ├── CustomAlertModal.jsx     NEW: Bottom-sheet modal for editing per-beach notification rules
│   │   │                              - RangeSlider component (dual-handle, snappable)
│   │   │                              - CompassArc component (SVG circle, 360° draggable handles)
│   │   │                              - Chips for tide/match-rule selection
│   │   │                              - Unit conversion (feet/knots) based on props
│   │   ├── WeatherIcon.jsx          Glass-style SVG weather icons (dark/light, day/night)
│   │   ├── Arrows.jsx               SwellArrow + WindArrow SVG components
│   │   └── ShareCard.jsx            Render-to-image share view
│   └── utils/
│       ├── customAlerts.js          NEW: Custom notification rule storage & matching
│       │                              - DEFAULT_RULE shape with all conditions
│       │                              - findMatchingWindows() scans forecast for contiguous matches
│       │                              - checkCustomAlerts() fires notifications with formatted body
│       │                              - inArc() handles direction wraparound (e.g., 315→45 for NW)
│       ├── formatting.js            degToCompass, formatHour, formatHeight, formatSwell, formatWind
│       ├── geo.js                   getDistanceKm (Haversine), getQualityScore
│       ├── moonPhase.jsx            getMoonPhase (Conway) + MoonIcon component (must be .jsx)
│       ├── conditions.js            getConditionsNow() — quality rating, wind type, tide state
│       └── forecast.js              fetchForecast() + generateOfflineData() fallback
├── ios/App/
│   ├── App.xcodeproj/
│   │   └── project.pbxproj          MARKETING_VERSION: 1.2, CURRENT_PROJECT_VERSION: 10
│   ├── App/
│   │   └── Info.plist               + ITSAppUsesNonExemptEncryption: false
│   └── Resources/
│       └── RoutingAppCoverageFile.geojson  NEW: GeoJSON with 6 AU coastal state polygons
├── public/, dist/                   Static + build output
├── capacitor.config.json            StatusBar style Dark, SplashScreen config
└── index.html                       Has viewport-fit=cover, apple-mobile-web-app-status-bar-style: black-translucent
```

---

## Color palette (dark theme)

| Token   | Hex       | Used for                          |
| ------- | --------- | --------------------------------- |
| bg      | `#0b0b1e` | App background                    |
| tide    | `#3dd6c8` | Tide data, "Glassy" rating        |
| swell   | `#8b9cf0` | Swell data, "Clean" rating        |
| wind    | `#e8916b` | Wind data, "Choppy" rating        |
| warn    | `#e04848` | Warnings, "Blown Out" rating      |

Light theme bg: `#f0ebe0`. **Important**: never compare `t.bg === "#0a0a18"` — that's a stale value. Use `"#0b0b1e"` or pass a `darkMode` boolean prop.

---

## Forecast / data architecture (`utils/forecast.js`)

The forecast system is built for **consistency under flaky networks**:

1. **Pinned model**: ECMWF IFS 0.25° (`models=ecmwf_ifs025`). Open-Meteo's `best_match` was selecting GFS for Australian coordinates and running ~3°C high vs Apple Weather; ECMWF matches Apple closely.
2. **Model fallback chain**: ECMWF → GFS Seamless. If ECMWF is briefly down, GFS keeps real data flowing instead of dropping to synthetic.
3. **In-memory cache**: `forecastCache` Map, 15-min TTL, keyed by `lat,lng` rounded to 3 decimals.
4. **In-flight dedup**: `inFlight` Map prevents two concurrent requests for the same beach.
5. **localStorage persistence**: every successful fetch is mirrored to `cc_fc_<key>`. Cold start with no network = real (≤15min) data instantly.
6. **1-hour stale fallback**: if both models fail AND no fresh cache, use persisted data up to 1h old before dropping to synthetic. (Was 6h, reduced to avoid stale icon display.)
7. **Validation guard**: `isValidWeatherData` rejects responses where >20% of hourly values are null.
8. **Synthetic fallback** (`generateOfflineData`): seeded harmonic approximation. Returned but **not cached** so the next call retries the live API.
9. **Tides are synthetic** — harmonic approximation in `forecast.js`. Real tide API is **not** integrated yet.
10. **Shark data is mock** — `data/sharks.js`. Needs real API (SharkSmart/Dorsal).

---

## State / persistence

LocalStorage keys:

| Key                 | Purpose                            |
| ------------------- | ---------------------------------- |
| `cc_use12h`         | 12h vs 24h time format              |
| `cc_useFeet`        | Feet vs metres for swell/wave      |
| `cc_useKnots`       | Knots vs km/h for wind             |
| `cc_darkMode`       | Dark vs light theme                 |
| `cc_favs`           | Favourited beach IDs (array)       |
| `cc_fc_<lat>,<lng>` | Persisted forecast (set by forecast.js) |

`SettingsMenu` component is shared between home and detail views.

---

## Custom Alerts System (`utils/customAlerts.js`)

**Rule Shape:**
```javascript
{
  mode: 'default|custom',
  match: 'all|any',                  // AND/OR logic
  swellMin: 0.5, swellMax: 3,        // metres
  periodMin: 8, periodMax: 16,       // seconds
  windMin: 5, windMax: 20,           // knots
  windDirectionArc: {start, end},    // degrees (0-360), handles wraparound
  swellDirectionArc: {start, end},   // degrees (0-360)
  tidePreference: 'any|low|mid|high',
  leadDays: 1-7,                     // days advance notice
  notifyDayOf: true                  // notify same-day window
}
```

**Key Functions:**
- `findMatchingWindows(forecast, rule)` — Returns contiguous forecast blocks matching all conditions
- `checkCustomAlerts(favData)` — Fires notifications with formatted time window + conditions
- `inArc(direction, start, end)` — Checks if direction is within arc, handles wraparound (e.g., NW = 315°–45°)
- `circularMean(directions)` — Averages direction values correctly (not arithmetic mean)

**Direction Arc Picker:**
- SVG circle with N/E/S/W labels
- Two draggable handles for arc start/end
- "Any direction" toggle disables constraint
- Full 360° support with visual feedback

**Storage:**
- Rules stored per beach in localStorage: `cc_alerts_<beachId>`
- Survives app restart and reinstall

---

## iOS specifics

- **Scroll lock**: `html, body { overflow: hidden; position: fixed }` and `#root { overflow-y: auto; overscroll-behavior: contain }` in `index.css`. Without this, iOS rubber-band drags the fixed top/bottom bars.
- **Safe area insets**: top/bottom bars use `padding: env(safe-area-inset-*)` so iOS clock/battery and home indicator never overlap content. Wind map header uses `calc(env(safe-area-inset-top) + 12px)`.
- **Input auto-zoom prevention** (NEW): All input/textarea/select have `font-size: 16px` minimum in `index.css`. iOS auto-zooms on focus if font < 16px.
- **Smooth wheel scroll with nested detection** (FIXED): `main.jsx` installs a `wheel`-event lerp (ease 0.4) for desktop/trackpad. Now detects nested scrollable elements (overflow: auto/scroll) and skips preventDefault() if they can consume the scroll. iOS native touch momentum is preserved.
- **Modal scroll support** (FIXED): CustomAlertModal uses `WebkitOverflowScrolling: touch` + `overscrollBehavior: contain` for native iOS scrolling on modals.
- **Wind map resize on layout change** (FIXED): New `useEffect([searchOpen])` calls `mapInstance.invalidateSize()` + custom `resizeFnRef.current()` via `requestAnimationFrame`. Fixes iOS glitch when search panel appears/disappears.
- **App icon (Asset Catalog)**: must be opaque PNG, no alpha channel. App Store rejects with error 90717 otherwise. Master icon at `ios/App/App/Assets.xcassets/AppIcon.appiconset/C4ST_MasterIcon.png`. If re-exported from Icon Composer, flatten alpha via Swift+CoreGraphics (PIL/sips alone won't work).

---

## Wind map (`components/WindMap.jsx`)

- Opens at user's geolocation (Capacitor `Geolocation.getCurrentPosition` on native; browser API on web). If permission denied, falls back to beach centroid and hides the "you are here" dot.
- Header is just "WIND MAP" + current speed/compass + Close button (no beach name). Safe-area-inset padding for iOS status bar.
- Tappable beach markers for up to 60 beaches within 250km of map centre. Click → `flyTo` + reload wind data for new centre.
- Velocity layer is a custom canvas particle system. `addVelocityLayer` returns a cleanup function stored in `velocityCleanupRef` to prevent stacking on marker switch.
- Default Leaflet zoom controls removed.
- Timeline scrubber bottom: `calc(env(safe-area-inset-bottom) + 96px)`.
- Wind speed legend bottom: `calc(env(safe-area-inset-bottom) + 48px)` — sits snug under timeline.
- 6×6 grid wind probe with `AbortSignal.timeout(6s)` and 30-min cache.

---

## Build / release flow

1. Edit code; verify locally (`npx vite --host` or via Claude preview).
2. For user-facing releases, bump both:
   - **Marketing version** (`MARKETING_VERSION`) — e.g., 1.0 → 1.2
   - **Build number** (`CURRENT_PROJECT_VERSION`) — increment by 1
3. Edit `ios/App/App.xcodeproj/project.pbxproj` — **two** places for each variable (Debug + Release configs), both must match.
4. `npm run build && npx cap sync ios`.
5. Add GeoJSON/config files to Xcode (if new): Select App target → Build Phases → Copy Bundle Resources → + button.
6. `open ios/App/App.xcodeproj`.
7. Xcode → select "Any iOS Device (arm64)" (not a specific device) → Product → Archive → Distribute App → App Store Connect → Upload.

**Current Build:** v1.2, Build 10 (ready for submission)

---

## Recent Changes (Apr 16-17, 2026)

### v1.2 Release Build

**Features Added:**
- **CustomAlertModal.jsx** — New bottom-sheet modal for per-beach notification rules
  - RangeSlider component (dual-handle, snappable, unit-aware)
  - CompassArc component (SVG circle, draggable handles, 360° arc selection)
  - Tide/match-rule selector chips
  - Accepts useFeet/useKnots props for dynamic unit display
- **customAlerts.js** — New utility module for rule storage & matching
  - `findMatchingWindows()` scans 7-day forecast for matching conditions
  - `checkCustomAlerts()` fires notifications with formatted time window
  - `inArc()` handles direction wraparound (e.g., NW = 315°–45°)
  - Rules stored per beach in localStorage

**iOS Fixes:**
- **Input auto-zoom fix** — Added `font-size: 16px` to all input/textarea/select in `index.css`
- **Modal scroll fix** — Fixed global wheel listener in `main.jsx` to detect nested scrollable elements and skip preventDefault() if they can consume scroll
- **Wind map resize fix** — Added `useEffect([searchOpen])` in WindMap.jsx calling `mapInstance.invalidateSize()` via requestAnimationFrame

**App Store Compliance:**
- **RoutingAppCoverageFile.geojson** — Created and added to `ios/App/Resources/`
  - GeoJSON FeatureCollection with 6 Australian coastal state polygons
  - Covers NSW, QLD, VIC, SA, WA, TAS
- **Info.plist** — Added `ITSAppUsesNonExemptEncryption: false` to avoid future encryption questionnaires

**Version & Build:**
- Bumped MARKETING_VERSION: 1.0 → 1.2
- Bumped CURRENT_PROJECT_VERSION: 9 → 10
- Updated both Debug and Release configs in project.pbxproj

**Website:**
- **c4st-website** — Complete redesign matching app aesthetic
  - Landing page with feature showcase
  - /support page with FAQ + contact form (Formspree integration)
  - Sitemap.xml for SEO
  - Live at https://c4st-website.vercel.app

**Git:**
- All changes committed to GitHub (main branch)
- Commits: GeoJSON file, version bump, encryption flag, website files, sitemap

---

### Previous Session (Apr 9-10)
- Graph.jsx — Added bare chevron arrows for carousel navigation
- App.jsx — Day strip alignment fix
- forecast.js — STALE_FALLBACK_MS reduced from 6h to 1h
- PRIVACY.md — Created for App Store requirement
- App icon — Re-exported as full-bleed 1024×1024

---

## Known gotchas / "don't repeat this mistake"

- **`moonPhase.jsx` MUST be `.jsx`** — it contains JSX. Not `.js`.
- **Don't compare against `#0a0a18`** to detect dark mode. The dark bg is `#0b0b1e`. There were two of these stale comparisons in `DayDetail.jsx` (WindMapPreview and the conditions WeatherIcon) — both fixed.
- **Don't use Open-Meteo `best_match`** — it picks GFS for AU. Always pin a model.
- **Don't skip the alpha-flatten step on app icons.** Icon Composer exports PNGs with alpha, App Store rejects.
- **Node not in PATH** — prefix with `export PATH="/opt/homebrew/bin:$PATH"` before npm/npx.
- **Two `CURRENT_PROJECT_VERSION` lines** in `project.pbxproj` (Debug + Release configs). Both must be bumped together.
- **In-memory cache survives HMR but not full reload**; localStorage persistence covers full reload.

---

## Website (c4st-website)

**Live URL:** https://c4st-website.vercel.app  
**Deployment:** Vercel (auto-deploy from GitHub)  
**Repository:** https://github.com/smoled/c4st-website

### Pages

1. **Home (index.html)**
   - Hero section with app download CTA
   - Beach card mockups showing key features
   - Feature grid highlighting: unified forecasts, custom alerts, wind map, scoring
   - Screenshots of custom alert UI (range sliders, compass arc picker)
   - Best time to surf section
   - Download CTA button

2. **Support (/support)**
   - FAQ accordion with 9 items covering common questions
   - Contact form (Formspree integration) directs to personal email
   - Professional styling matching app design

### Design System
- **Colors:** Matching app theme (#0b0b1e background, glass surfaces)
- **Fonts:** Outfit (headings), JetBrains Mono (data labels)
- **Components:** Cards with hover effects, responsive grid layouts
- **SEO:** sitemap.xml with home (priority 1.0) and support (priority 0.8)

---

## Next Steps for App Store Submission

1. **Archive the app:**
   ```bash
   open ios/App/App.xcodeproj
   # Select "Any iOS Device (arm64)" from scheme dropdown
   # Product → Archive
   ```

2. **Upload to App Store Connect:**
   - Archive is ready for distribution
   - Select "Distribute App" → "App Store Connect" → "Upload"
   - Xcode will verify and upload the build

3. **Complete app metadata:**
   - App Category: Weather
   - Promotional text (see above)
   - Screenshots (iPhone + iPad)
   - Privacy policy: https://raw.githubusercontent.com/smoled/coastcast/main/PRIVACY.md
   - Support URL: https://c4st-website.vercel.app/support

4. **Compliance checklist:**
   - [ ] Content Rights — Declare in App Information
   - [ ] App Privacy — Complete questionnaire (no location tracking outside app scope, only for Nearby Spots feature)
   - [ ] Encryption — Already declared as non-exempt in Info.plist

5. **Submit for Review:**
   - All items complete → Click "Add for Review"
   - Apple will notify of approval/rejection

---

## Useful Commands

```bash
# Development
cd "/Users/sol/Documents/Claude/Tidal APP/coastcast"
export PATH="/opt/homebrew/bin:$PATH"
npx vite --host                      # dev server on :5173

# Build & sync
npm run build && npx cap sync ios    # build web + sync to iOS

# Open Xcode
open ios/App/App.xcodeproj

# Git operations
git status
git add <files>
git commit -m "message"
git push origin main
```

---

## Original prototype

The monolithic prototype is at `/Users/sol/Documents/Claude/Tidal APP/tide app/coastcast.jsx` with project plan in `projectplan.md` next to it.

---

**Last Updated:** April 17, 2026  
**Status:** ✅ Ready for App Store Submission  
**Version:** 1.2 (Build 10)
