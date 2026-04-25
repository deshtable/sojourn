# Sojourn ✈️

> A refined trip planning app — plan your journey, mark hotels, and visualize your route on a live map.

**Live app:** [deshtable.github.io/sojourn](https://deshtable.github.io/sojourn)

---

## Features

- **Google Sign-In** — secure authentication, trips saved to your account
- **Multi-trip management** — create and switch between multiple trips
- **Destination planning** — add stops, set arrival/departure dates, drag to reorder
- **Hotel tracking** — mark hotels at any stop with name and number of nights
- **Live route map** — Google Maps integration showing actual driving roads between all stops
- **PDF export** — clean printable itinerary
- **Auto-save** — all changes saved to Firebase in real time

## Tech Stack

- **Frontend** — Vanilla HTML/CSS/JS (single file, no build step)
- **Auth** — Firebase Authentication (Google Sign-In)
- **Database** — Cloud Firestore
- **Maps** — Google Maps Embed API + Directions API
- **Hosting** — GitHub Pages

## Setup (for your own deployment)

1. Clone this repo
2. Create a [Firebase project](https://console.firebase.google.com) and enable Google Auth + Firestore
3. Create a [Google Maps API key](https://console.cloud.google.com) with Maps JavaScript API + Directions API enabled
4. Replace the `firebaseConfig` and `MAPS_KEY` values in `index.html`
5. Push to GitHub and enable GitHub Pages

## License

MIT — free to use and modify.
