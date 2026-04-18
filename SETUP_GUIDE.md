# Evalia — Setup & Operations Guide

## 1. First-Time Setup

### Connect the Admin Panel

1. Go to `evalia-web.vercel.app/admin.html`
2. Generate a GitHub Personal Access Token:
   - Visit [github.com/settings/tokens/new](https://github.com/settings/tokens/new)
   - Name: "Evalia Admin" | Expiration: No expiration
   - Scope: tick **repo** (full access)
   - Click Generate — copy the token
3. Paste the token in the admin Connect page
4. The admin stores the token in your browser's localStorage. You only enter it once per browser.

---

## 2. Editing Content

### Properties
1. Admin → **Properties** → click **Edit**
2. Fill in both 🇬🇧 English and 🇫🇷 French fields side-by-side
3. Add photos and videos to the **Gallery** — click **+** to open the media picker
4. The first gallery item becomes the cover image on the site
5. Click **Save & Publish →** — Vercel rebuilds in ~30 seconds

### Experiences
Same workflow as properties. Each experience also has a **Category** (boat / nature / food / concierge) which controls which filter tab it appears under.

### Settings
Admin → **Settings** to update:
- Business name, email, WhatsApp, social handles
- Hero section text (bilingual — EN and FR side-by-side)
- Hero slideshow photos (4 slides)
- Service card photos (6 service cards)

---

## 3. Uploading Media

Admin → **Media Library**:

- Drag and drop files or click to browse
- Multiple files upload in parallel (3 at a time)
- Each file shows its own progress bar
- JPG, PNG, WebP, MP4 and MOV supported — max 50 MB per file
- After upload, files appear in the media library grid immediately
- Use the **📷** button on any photo field to open the media picker and assign photos

To **assign a photo to a property or experience**: open the item in Properties or Experiences, click the 📷 button next to the photo field, and select or upload from the modal.

To **assign hero or service card photos**: go to Settings and use the photo fields there.

---

## 4. Language Toggle (EN / FR)

The site has a 🇬🇧/🇫🇷 flag toggle button in the navigation bar.

- Visitors click it to switch between English and French
- Their preference is saved in the browser (`localStorage`)
- Static UI text switches immediately via the `I18N` dictionary
- Property and experience names, descriptions, and durations switch based on the `*_fr` fields in the JSON

To ensure content appears correctly in French: always fill in the `*_fr` fields when editing properties and experiences in the admin panel.

---

## 5. Gallery & Lightbox

Each property and experience supports multiple photos and videos:

- Add photos via the **Gallery** section in the admin edit form
- Videos (MP4) are supported alongside photos
- On the website, if an item has more than one photo, a thumbnail strip appears below the cover image
- Clicking any thumbnail opens a full-screen lightbox with:
  - Previous / Next arrows
  - Dot navigation
  - Keyboard support: ← → to navigate, Esc to close
  - Swipe support on mobile

---

## 6. Booking Flow

1. Guests select services in Step 1 of the booking wizard
2. Fill dates, name, email, phone in Step 2
3. Review summary in Step 3 and click **Send Booking Request**
4. Request is submitted to Formspree (endpoint: `mdayoknj`)
5. You receive an email; follow up via WhatsApp or email within 2 hours

To view all submissions: [formspree.io/forms/mdayoknj/submissions](https://formspree.io/forms/mdayoknj/submissions)

---

## 7. SHA Mismatch / Write Errors

If a "Save failed" error appears in admin:
1. Click **Refresh** in the topbar
2. Re-open the item and edit again
3. Save again

The admin always fetches a live SHA from GitHub immediately before every write, preventing conflicts.

---

## 8. Adding a New Property or Experience

1. Admin → Properties (or Experiences) → click **+ Add**
2. A new item opens in edit mode at the bottom of the list
3. Fill in all fields including EN and FR text
4. Add gallery photos
5. Click **Save & Publish →**
6. The manifest (`_data/manifest.json`) auto-updates so the new item appears on the live site

---

## 9. Contacts & Access

| Resource | Value |
|---|---|
| Live site | https://evalia-web.vercel.app |
| Admin panel | https://evalia-web.vercel.app/admin.html |
| GitHub repo | https://github.com/cmarcdavid/evalia-web |
| Formspree | https://formspree.io/forms/mdayoknj/submissions |
| WhatsApp | +230 5724 2099 |
| Email | hello@evalia.mu |
| Instagram | @evalia241 |
