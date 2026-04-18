# Evalia Mauritius — Architecture & CMS Guide

## Overview

Evalia is a Vercel-hosted static site backed by a GitHub-native CMS.
All content lives as JSON in `_data/`. Admin writes to GitHub via the REST API.
Vercel auto-deploys on every commit (~30 seconds).

```
Admin Panel (admin.html)
      │
      ▼  GitHub REST API (live SHA on every write)
GitHub repo  cmarcdavid/evalia-web
      │
      ▼  Vercel auto-deploy on push
Live site   evalia-web.vercel.app
      │
      ▼  fetch() at runtime
index.html reads _data/manifest.json → _data/**/*.json
```

---

## Data Schema

### Properties (`_data/properties/<slug>.json`)

```json
{
  "name":          "Villa Evalia",
  "name_fr":       "Villa Evalia",
  "type":          "Villa",
  "location":      "Grand Baie, North Mauritius",
  "location_fr":   "Grand Baie, Nord de l'Île Maurice",
  "description":   "English description…",
  "description_fr":"French description…",
  "price":         180,
  "max_guests":    6,
  "bedrooms":      2,
  "bathrooms":     2,
  "min_stay":      3,
  "cleaning_fee":  60,
  "photo":         "/images/Villa-pool.jpg",
  "gallery":       ["/images/Villa-pool.jpg", "/images/Villa-pool1.jpg"],
  "badge":         "Pool & Garden",
  "badge_fr":      "Piscine & Jardin",
  "available":     true,
  "amenities":     ["Pool","Garden","WiFi","Kitchen","Parking","Air Con"]
}
```

### Experiences (`_data/experiences/<slug>.json`)

```json
{
  "name":           "Dolphin Watching Cruise",
  "name_fr":        "Croisière Observation des Dauphins",
  "category":       "boat",
  "icon":           "🐬",
  "description":    "English description…",
  "description_fr": "French description…",
  "duration":       "Half day",
  "duration_fr":    "Demi-journée",
  "price":          45,
  "unit":           "person",
  "gallery":        ["/images/photo1.jpg", "/images/video1.mp4"],
  "instagram":      "",
  "active":         true
}
```

**gallery** array accepts JPG, PNG, WebP and MP4/MOV files.
The first item is the cover image. The website renders a clickable thumbnail strip
and opens a full-screen lightbox on click.

### Settings (`_data/settings.json`)

```json
{
  "name":                    "Evalia Mauritius",
  "whatsapp":                "+23057242099",
  "email":                   "hello@evalia.mu",
  "instagram":               "@evalia241",
  "hero_line1":              "Your Mauritius,",
  "hero_line1_fr":           "Votre Maurice,",
  "hero_line2":              "Perfectly Arranged",
  "hero_line2_fr":           "Parfaitement Orchestré",
  "hero_sub":                "English subtitle…",
  "hero_sub_fr":             "French subtitle…",
  "hero_photo":              "/images/Hero.jpg",
  "hero_photo2":             "/images/Hero1.jpg",
  "hero_photo3":             "/images/Hero2.jpg",
  "hero_photo4":             "/images/Hero3.jpg",
  "svc_accommodation_photo": "/images/Hero1.jpg",
  "svc_boat_photo":          "/images/Hero2.jpg",
  "svc_nature_photo":        "/images/Hero3.jpg",
  "svc_food_photo":          "",
  "svc_concierge_photo":     "/images/Host.jpg"
}
```

### Manifest (`_data/manifest.json`)

Auto-updated by admin on every save/delete. Tells `index.html` which slugs to fetch.

```json
{
  "properties":  ["studio-evalia","villa-evalia"],
  "experiences": ["dolphin-watching-cruise","sunrise-beach-yoga","..."]
}
```

---

## Multi-language (EN / FR)

**Website:** A flag toggle button in the nav switches between English and French.
The choice is stored in `localStorage('ev_lang')`.

- Static text: controlled by the `I18N` dictionary in `index.html` JS, applied by `applyLang()`.
- CMS content: each JSON record stores `name_fr`, `description_fr`, `location_fr`, `duration_fr`, `badge_fr`. `renderExp()` and `renderProperties()` read the correct field based on `lang`.

**Admin panel:** All edit forms show bilingual fields side-by-side (🇬🇧 / 🇫🇷) so both languages are always edited together.

---

## Media Upload Performance

Uploads fire in parallel batches of 3 concurrent requests (`CONC = 3`).
Each file shows individual progress in the upload queue UI (optimistic).

Flow per file:
1. FileReader → base64 (async)
2. Fetch live SHA from GitHub (non-blocking)
3. PUT to `images/<filename>` via GitHub Contents API
4. Progress bar updates: 30% (read done) → 55% (SHA fetched) → 100% (PUT complete)

After all uploads complete, the media list refreshes automatically
and all open edit forms re-populate their photo dropdowns.

---

## Gallery & Lightbox

Each property and experience has a `gallery` array in JSON.

- **Admin:** Gallery manager shows a grid of thumbnails. Click `+` to open the photo picker modal (upload new or select from library). Click `✕` on a thumbnail to remove. The first item auto-becomes the cover photo.
- **Website:** If gallery has >1 item, a horizontal thumbnail strip renders below the card image. Clicking any thumbnail (or the cover photo) opens the full-screen lightbox with prev/next navigation and keyboard support (←, →, Esc).

---

## Key Files

| File | Purpose |
|---|---|
| `index.html` | Live site — fetches `_data/*.json`, renders content, EN/FR toggle, lightbox |
| `admin.html` | CMS panel — reads/writes GitHub JSON, gallery management, parallel uploads |
| `_data/manifest.json` | Auto-updated list of content slugs |
| `_data/settings.json` | Site-wide settings: text, photos, contacts |
| `_data/properties/` | One JSON file per property |
| `_data/experiences/` | One JSON file per experience |
| `images/` | All media files (uploaded via admin) |
| `site.webmanifest` | PWA manifest |

---

## Deployment

```
GitHub Desktop → Commit → Push → Vercel auto-builds in ~30s
```

No build step. Vercel serves static files directly.
