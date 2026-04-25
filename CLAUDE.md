# Sojourn — Claude Project Context

This file gives Claude full context about the Sojourn project so it can assist without needing prior conversation history.

---

## What is Sojourn?

Sojourn is a personal trip planning web app. It is a single-file static app (`index.html`) hosted on GitHub Pages. Users sign in with Google, create trips, add destinations, plan hotels, view their route on Google Maps, and export a PDF itinerary.

**Live URL:** https://deshtable.github.io/sojourn  
**GitHub:** https://github.com/deshtable/sojourn  
**Owner:** deshtable (Desh)

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML + CSS + JavaScript (no framework, no build step) |
| Auth | Firebase Authentication — Google Sign-In |
| Database | Cloud Firestore |
| Maps | Google Maps Embed API + Directions API |
| Hosting | GitHub Pages (push to main = deploy) |

Everything lives in a single `index.html` file. There is no bundler, no npm, no node_modules.

---

## Firebase Config

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyCndav3VNI4wGt7OIFuIBsbfOkr72VlYJY",
  authDomain: "sojourn-aa2d1.firebaseapp.com",
  projectId: "sojourn-aa2d1",
  storageBucket: "sojourn-aa2d1.firebasestorage.app",
  messagingSenderId: "1088310534555",
  appId: "1:1088310534555:web:7627fff0c5b28df34f413e",
  measurementId: "G-DR1SD3NT2K"
};
```

Firebase project name: **Sojourn** (Spark plan — free tier)  
Firebase Auth: Google Sign-In enabled  
Firestore: enabled, currently in test mode

---

## Google Maps API Key

```
AIzaSyCxuOLOzSFbrsqk9LTi92A5aPP1zYQEQWo
```

APIs enabled: Maps JavaScript API, Directions API  
Restriction: currently unrestricted (add `https://deshtable.github.io/*` when ready)

---

## App Architecture

### Data model (Firestore)
```
users/{userId}/trips/{tripId}
  - name: string
  - createdAt: timestamp
  - updatedAt: timestamp

users/{userId}/trips/{tripId}/destinations/{destId}
  - name: string
  - arrival: string (date, YYYY-MM-DD)
  - departure: string (date, YYYY-MM-DD)
  - hasHotel: boolean
  - hotelName: string
  - nights: number
  - notes: string
  - order: number (for drag-and-drop ordering)
```

### Key JS state variables
- `currentUser` — Firebase auth user object
- `trips` — array of all user's trips
- `currentTripId` — ID of active trip
- `destinations` — array of destination objects for current trip
- `activeTab` — 'destinations' | 'hotels'
- `showAddForm` — boolean, controls add form visibility

### Key functions
- `loadTrips()` — loads all trips from Firestore on sign-in
- `loadTrip(tripId)` — loads destinations for a specific trip
- `saveCurrentTrip()` — batch saves all destinations to Firestore
- `scheduleSave()` — debounced save (1.2s after last change)
- `showRoute()` — builds Google Maps embed URL and displays it
- `exportPDF()` — triggers window.print() with print CSS
- `render()` — re-renders the sidebar content
- `renderAll()` — render + updateSummary + updateMapBadge

---

## Design System

**Fonts:** Cormorant Garamond (display/headings) + Jost (body/UI)  
**Aesthetic:** Refined, editorial, luxury travel — warm neutrals, sage green accents

### CSS Variables
```css
--ink: #1C1917          /* primary text */
--ink-soft: #44403C     /* secondary text */
--muted: #A8A29E        /* placeholder/labels */
--border: #E7E5E4       /* default borders */
--border-dark: #D6D3D1  /* emphasized borders */
--cream: #FAFAF9        /* page background */
--white: #FFFFFF
--sage: #4D6B53         /* primary accent (buttons, active states) */
--sage-light: #EDF3EE   /* sage backgrounds */
--sage-mid: #7A9E82     /* sage mid tone */
--rust: #B45309         /* badges */
--rust-light: #FEF3C7
--sky: #1D4ED8          /* hotel accent */
--sky-light: #EFF6FF
--danger: #DC2626
--danger-light: #FEF2F2
```

---

## Current Features

- ✅ Google Sign-In via Firebase Auth
- ✅ Multiple trips (create, switch, rename)
- ✅ Add/remove/reorder destinations (drag and drop)
- ✅ Arrival + departure date pickers per stop
- ✅ Hotel toggle per stop (name + nights)
- ✅ Notes per destination
- ✅ Hotels overview tab
- ✅ Google Maps route (driving, all waypoints)
- ✅ PDF export via print CSS
- ✅ Auto-save to Firestore with saving indicator
- ✅ Summary bar (stops, hotels, nights, trip type)

---

## Planned / Not Yet Built

- [ ] Collaborators / trip sharing
- [ ] Custom map marker types (flight, train, drive)
- [ ] Flight/transport details between stops
- [ ] Mobile-optimized layout
- [ ] Firestore security rules (currently in test mode)
- [ ] Budget tracker
- [ ] Packing list

---

## Deployment

**To deploy:** just push to `main` branch. GitHub Pages auto-deploys in ~60 seconds.

```bash
git add .
git commit -m "describe your change"
git push
```

**No build step required.** Edit `index.html` directly.

---

## Important Notes for Claude

- All code is in one file: `index.html`. Do not split into multiple files unless explicitly asked.
- Use the Firebase compat SDK (already loaded via CDN) — not the modular SDK.
- Keep the design consistent with the existing aesthetic — Cormorant Garamond + Jost, sage/cream palette.
- Auto-save is debounced — always call `scheduleSave()` after mutating `destinations`.
- Always call `renderAll()` after structural changes, `render()` for tab-only re-renders.
- The Maps embed uses the `v1/directions` endpoint which requires an API key.
- Firestore is currently in test mode — remind Desh to tighten rules before sharing with other users.
