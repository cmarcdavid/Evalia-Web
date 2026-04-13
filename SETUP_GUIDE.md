# Evalia Mauritius — Setup Guide v4
## Getting live in 30 minutes

---

## PART 1 — Formspree Forms (10 min, free)

You need 3 forms. Go to **formspree.io** → sign up → create each:

| Form Name | Replace in index.html |
|-----------|----------------------|
| Evalia Registration | `YOUR_FORMSPREE_REGISTER_ID` |
| Evalia Booking | `YOUR_FORMSPREE_BOOKING_ID` (appears 2×) |

### How to find and replace:
1. Open `index.html` in TextEdit (right-click → Open With → TextEdit)
2. Press **Cmd+F** → search: `YOUR_FORMSPREE`
3. Replace each one with your actual Formspree ID
4. Save with **Cmd+S**

---

## PART 2 — GitHub Desktop + Vercel (10 min)

1. Install **GitHub Desktop** from desktop.github.com
2. Create a new repo called `evalia`
3. Drag all files into the repo folder
4. Commit → Push
5. Go to **vercel.com** → New Project → Import GitHub repo → Deploy

Your site will be live at `evalia.vercel.app` (or your custom domain)

---

## PART 3 — Decap CMS (No-code admin panel — 5 min)

This replaces Cloudinary entirely. You get a visual admin at `/admin`.

### A. Fix the config
Open `admin/config.yml` and replace:
```
repo: YOUR_GITHUB_USERNAME/evalia
```
With your actual GitHub username, e.g.:
```
repo: christopherdavid/evalia
```

### B. Enable GitHub OAuth on Vercel
1. Go to **github.com → Settings → Developer Settings → OAuth Apps → New OAuth App**
2. Fill in:
   - Application name: `Evalia CMS`
   - Homepage URL: `https://evalia.vercel.app`
   - Authorization callback URL: `https://api.netlify.com/auth/done`
3. Copy the **Client ID** and **Client Secret**
4. Go to **vercel.com → Project → Settings → Environment Variables**
5. Add: `GITHUB_CLIENT_ID` = your client ID
6. Add: `GITHUB_CLIENT_SECRET` = your client secret

### C. Use the CMS
1. Go to `https://evalia.vercel.app/admin`
2. Click **Login with GitHub**
3. You'll see a visual dashboard to:
   - ✅ Add/edit properties with photo uploads
   - ✅ Add/edit experiences
   - ✅ Change site settings, hero text, phone number
   - ✅ Everything saves automatically to GitHub → Vercel redeploys in 30 seconds

---

## PART 4 — Add Photos Without CMS (simpler option)

If you don't want to set up the CMS yet, just add photos directly:

1. Drop any `.jpg` photo into the `images/` folder in your GitHub repo
2. In `index.html`, find the `<!-- ADD PHOTO -->` comment for each property
3. Uncomment the `<img>` tag and set `src="images/your-photo.jpg"`
4. Push → Vercel deploys → photo appears live

---

## PART 5 — WhatsApp Number

Search `23057000000` in `index.html` and replace with your real Mauritius number (no + or spaces).

---

## PART 6 — Payments (when ready for live bookings)

Add your Stripe keys in **Vercel → Settings → Environment Variables**:
- `STRIPE_PUBLISHABLE_KEY` = pk_live_...
- `STRIPE_SECRET_KEY` = sk_live_... (never in code!)

MyT Money and MCB Juice: the booking flow already sends requests via WhatsApp/email.
When you integrate directly, add the API keys the same way.

---

## Quick File Reference

| File | Purpose |
|------|---------|
| `index.html` | Main website — edit text/prices here |
| `admin/index.html` | CMS panel (do not edit) |
| `admin/config.yml` | CMS structure — edit repo name here |
| `images/` | Drop your photos here |
| `.env.example` | Environment variable template |
| `.gitignore` | Keeps secrets out of GitHub |
| `ARCHITECTURE.md` | Technical documentation |
| `PRD.md` | Product requirements |

---

*Evalia Mauritius — v4 — April 2026*
