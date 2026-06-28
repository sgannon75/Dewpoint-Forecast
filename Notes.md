# Dewpoint Forecast App — Project Summary
**Date:** June 28, 2026  
**Developer:** Sean Gannon (sgannon75)  
**Live URL:** [dewpoint-forecast.vercel.app](https://dewpoint-forecast.vercel.app)  
**GitHub:** [github.com/sgannon75/Dewpoint-Forecast](https://github.com/sgannon75/Dewpoint-Forecast)

---

## What We Set Out to Do

Sean wanted a simple weather app that emphasized **dewpoint temperature** and **relative humidity** — two metrics that better capture summer discomfort than standard temperature alone, but are chronically underreported by local meteorologists. The goal was a personal tool he could reference daily and share with family.

---

## What We Built

A fully deployed, live weather forecast web app with the following features:

- **5-day forecast** for any city or US zip code
- **Dewpoint temperature** displayed prominently in °F and °C
- **Relative humidity** with a color-coded bar (dry / comfortable / humid)
- **Feels Like** temperature using the Rothfusz heat index equation
- **Hourly chart** per day showing temp, feels like, and dewpoint curves (tap any day card to expand)
- **Dewpoint Discomfort Scale** legend (Comfortable → Noticeable → Sticky → Oppressive → Miserable)
- **Live data** from Open-Meteo (free, no API key required)
- Accessible from any device via browser, and installable on iPhone home screen

---

## Technology Stack

| Layer | Technology | Purpose |
|---|---|---|
| Frontend | React (JSX) | UI components and interactivity |
| Build tool | Vite | Bundles the React app for production |
| Weather data | Open-Meteo API | Free live forecast data (no API key needed) |
| Geocoding | Open-Meteo Geocoding API | Converts city names / zip codes to lat/lon |
| Backend function | Vercel Serverless Function (`api/forecast.js`) | Proxies Open-Meteo requests server-side |
| Hosting | Vercel | Free hosting with auto-deploy from GitHub |
| Source control | GitHub | Stores and versions the code |

---

## The Journey — Key Milestones & Lessons Learned

### Milestone 1: Building the App in Claude
- Built a React app inside Claude's chat interface (an "artifact")
- Initial version used **Open-Meteo API** called directly from the browser
- Learned: Claude's artifact sandbox **blocks external network requests**, causing "Load failed" errors

### Milestone 2: Attempting Workarounds Inside Claude
- Tried routing through the **Anthropic Claude API** with web search to fetch live data
- Encountered "Invalid response format" and "Load failed" errors due to sandbox restrictions
- Tried using **Claude tool use** (`submit_forecast`) to force structured JSON output
- Learned: Web search tools are not available inside artifact sandboxes; the environment has strict limitations

### Milestone 3: Switching to Climate Estimates
- Simplified approach: used Claude's built-in climate knowledge to generate seasonal estimates
- App worked inside Claude but data wasn't live (e.g., missed a real heat wave)
- Learned: AI-generated estimates are useful but can't replace live weather APIs for accuracy

### Milestone 4: Deploying to the Real Web
Walked through the full deployment pipeline step by step:

1. **Created a GitHub account** (sgannon75)
2. **Created a GitHub repository** (Dewpoint-Forecast)
3. **Added project files** one by one via GitHub's web interface:
   - `package.json` — defines the project and its dependencies
   - `vite.config.js` — configures the Vite build tool
   - `index.html` — the HTML entry point
   - `src/main.jsx` — React entry point
   - `src/App.jsx` — the full weather app
4. **Created a Vercel account** and connected it to GitHub
5. **Deployed with one click** — Vercel auto-detected the Vite/React setup

### Milestone 5: Connecting the Anthropic API Key
- Added `VITE_ANTHROPIC_API_KEY` as a Vercel environment variable (secure storage)
- Updated `App.jsx` to include the API key header in requests
- Learned: Never hardcode API keys in source code — always use environment variables

### Milestone 6: Switching to Live Data (The Final Fix)
- Realized the real problem: browser sandboxes block direct calls to external APIs
- Solution: Added a **Vercel serverless function** (`api/forecast.js`) as a middleman
- The browser calls `/api/forecast` → Vercel calls Open-Meteo → returns live data
- Learned: Server-side functions bypass browser restrictions entirely — this is the standard pattern for production apps
- Result: **Real live forecast data**, including the actual heat wave showing 103°F on Thursday!

### Milestone 7: Polish
- Renamed repository and Vercel project from `Dewpoint-Forecat` → `Dewpoint-Forecast`
- Updated live URL to `dewpoint-forecast.vercel.app`
- Removed the old misspelled domain

---

## Key Concepts Learned

### Why Dewpoint Matters More Than Humidity
Relative humidity tells you how "full" the air's moisture tank is — but that tank size changes with temperature. Dewpoint tells you the actual amount of moisture in the air regardless of temperature. A dewpoint of 68°F feels oppressive whether it's 75° or 95°. That's why meteorologists *should* be leading with it.

### The Discomfort Scale
| Dewpoint | Feel |
|---|---|
| < 55°F | Comfortable — pleasant air |
| 55–59°F | Noticeable — slight mugginess |
| 60–64°F | Sticky — noticeably humid |
| 65–69°F | Oppressive — heavy & uncomfortable |
| ≥ 70°F | Miserable — dangerous for exertion |

### Browser Sandbox vs. Server-Side
Think of it like a post office: your browser (the customer) can't walk into the back warehouse (external APIs) directly. A serverless function is like the counter clerk — the customer hands the request to the clerk, the clerk goes to the warehouse, and brings back the package.

### Environment Variables
API keys should never live in your code — anyone who views your source could steal them. Vercel's environment variables store them securely on the server, injected at build time.

### The GitHub → Vercel Pipeline
Every time you commit a change to GitHub, Vercel automatically rebuilds and redeploys your app. This is called **continuous deployment** — your live site always reflects your latest code within about 30 seconds.

---

## Files in the Project

```
Dewpoint-Forecast/
├── index.html          # HTML entry point
├── package.json        # Project dependencies
├── vite.config.js      # Build configuration
├── api/
│   └── forecast.js     # Serverless function (proxies Open-Meteo)
└── src/
    ├── main.jsx        # React entry point
    └── App.jsx         # Main app (all UI and logic)
```

---

## Ideas for Future Enhancements
- Add a precipitation forecast
- Add wind speed (affects feels-like in winter)
- Push notifications for high dewpoint days
- Save favorite locations
- Add a "this time last year" comparison

---

*Built with Claude on June 28, 2026*
