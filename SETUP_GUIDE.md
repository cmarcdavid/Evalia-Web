# Evalia — Setup Guide
## Two things to activate: Forms + Photos

---

## STEP 1 — Activate Email Forms (5 minutes, free)

Your booking form and contact form are wired up and ready.
You just need to connect them to Formspree so submissions arrive in your inbox.

### A. Create your Formspree account
1. Go to **https://formspree.io** → Sign up free
2. Click **"+ New Form"**
3. Name it **"Evalia Booking"** → click Create
4. Copy your endpoint — it looks like: `https://formspree.io/f/xwkgpqnj`

### B. Create a second form for contact
1. Click **"+ New Form"** again
2. Name it **"Evalia Contact"** → click Create
3. Copy this endpoint too

### C. Paste into index.html
Open `index.html` and find these two lines (search for `YOUR_FORMSPREE`):

```html
<!-- Line ~430: Booking form -->
<form id="bookingForm" action="https://formspree.io/f/YOUR_FORMSPREE_BOOKING_ID"

<!-- Line ~510: Contact form -->  
<form id="contactForm" action="https://formspree.io/f/YOUR_FORMSPREE_CONTACT_ID"

<!-- Line ~545: Modal booking form -->
<form id="modalForm" action="https://formspree.io/f/YOUR_FORMSPREE_BOOKING_ID"
```

Replace `YOUR_FORMSPREE_BOOKING_ID` and `YOUR_FORMSPREE_CONTACT_ID` with your actual IDs.

### D. Deploy
Push the updated `index.html` to GitHub → Vercel auto-deploys in ~30 seconds.

### What you'll receive per booking request:
```
From: guest@email.com
Subject: New Booking Request — Evalia

name: Sophie Laurent
email: sophie@email.com
phone: +33 6 12 34 56 78
property: Villa Azura — Coastline
checkin: 2025-06-12
checkout: 2025-06-18
guests: 2 Guests
estimated_total: €2,365
```

---

## STEP 2 — Add Real Property Photos & Videos

Your site has photo/video slots ready. You just need to drop in your URLs.

### Option A — Use Cloudinary (recommended, free tier)
1. Go to **https://cloudinary.com** → sign up free
2. Upload your photos/videos to the Media Library
3. Click any asset → Copy URL
4. URLs look like: `https://res.cloudinary.com/YOUR_CLOUD/image/upload/v1/evalia/photo.jpg`

### Option B — Use your Instagram photos
1. Open your Instagram photo on desktop
2. Right-click the image → "Copy image address"
3. Use that URL directly

### Option C — Any direct image URL
Any public image URL works (Google Drive shared link, Dropbox, etc.)

---

### How to add photos to a property

Open `index.html` and find the properties array (~line 600).
Each property has a `photos` array. Add your URLs:

```javascript
{
  id: 1, name: 'Villa Azura', ...
  photos: [
    'https://res.cloudinary.com/evalia/image/upload/v1/azura-pool.jpg',
    'https://res.cloudinary.com/evalia/image/upload/v1/azura-bedroom.jpg',
    'https://res.cloudinary.com/evalia/image/upload/v1/azura-terrace.jpg',
  ],
  videoUrl: 'https://res.cloudinary.com/evalia/video/upload/v1/azura-tour.mp4',
  hasVideo: true,
}
```

- **First photo** = shown on the card thumbnail
- **All photos** = open in the gallery lightbox when clicked
- **videoUrl** = plays in the video lightbox when "▶ Video Tour" is clicked

### How to add the hero background photo

Find this comment near line ~200 in index.html:

```html
<!--
  PHOTO SLOT: To add your hero background image, uncomment this line and set the URL:
  <div class="hero-photo" style="background-image:url('YOUR_IMAGE_URL_HERE')"></div>
-->
```

Uncomment it and add your hero image URL.

---

## STEP 3 — Update your WhatsApp number

Find and replace `24106000000` with your actual WhatsApp number (no + or spaces):

- Line ~130: `<a class="wa-float" href="https://wa.me/24106000000"`
- Line ~510: `<a href="https://wa.me/24106000000"`
- Line ~560: `<a href="https://wa.me/24106000000"`

---

## Quick Checklist

- [ ] Create Formspree account and replace the 2 endpoint IDs
- [ ] Upload photos to Cloudinary and add URLs to properties array
- [ ] Upload videos to Cloudinary and add URLs
- [ ] Add hero background photo
- [ ] Update WhatsApp number
- [ ] Push to GitHub → Vercel deploys automatically

That's it — your site will be fully operational! 🚀

---

*Questions? Email hello@evalia.co or WhatsApp +241 06 00 00 00*
