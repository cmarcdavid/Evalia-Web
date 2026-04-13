# Evalia Mauritius — Platform Architecture v4.0
## "Your Mauritius, Perfectly Arranged"

---

## Vision
Evalia is a one-stop concierge platform for tourists visiting Mauritius, offering:
- Accommodation (Studio + 2-bed villa with pool/garden)
- Boat excursions & water activities
- Nature excursions & island tours
- Local food discovery experiences
- Wide-ranging concierge services (healthcare, transport, safety)
- Guest registration, booking, scheduling & payment

---

## CMS: Netlify CMS (now Decap CMS) — FREE, No-Code, Git-based

### Why Netlify/Decap CMS over Cloudinary
| Feature | Cloudinary | Decap CMS |
|---------|-----------|-----------|
| Photo upload | ✅ | ✅ built-in |
| Edit text/prices | ❌ need code | ✅ visual editor |
| Add new services | ❌ need code | ✅ no-code |
| Manage bookings view | ❌ | ✅ |
| Cost | Free (25GB) | Free forever |
| Technical skill needed | Medium | None |
| Works with Vercel+GitHub | Partial | ✅ Native |

### How it works
1. You go to: yourdomain.com/admin
2. Log in with your GitHub account (one click)
3. See a visual CMS — edit property descriptions, prices, photos, services
4. Click Save → automatically pushes to GitHub → Vercel deploys in 30 seconds
5. No code. No command line. No URL copying.

---

## Tech Stack

| Layer | Technology | Why |
|-------|-----------|-----|
| Frontend | HTML5 / CSS3 / Vanilla JS | Zero dependencies, fast |
| CMS | Decap CMS (Netlify CMS) | Free, Git-based, no-code admin |
| Forms/Bookings | Formspree | Email + submissions |
| Payments | Stripe + MyT Money (Mauritius) | Local + international |
| Hosting | Vercel | Free, auto-deploy |
| Version Control | GitHub | Auto-deploys to Vercel |
| Media | Decap CMS media uploads | Direct to GitHub repo |

---

## Site Structure

```
/                     → Home (hero, services overview, testimonials)
/accommodation        → Studio + Villa listings with booking
/excursions           → Boat, nature, food discovery
/services             → Concierge, healthcare, transport, safety
/booking              → Unified booking + scheduling system
/register             → Guest registration
/payment              → Checkout with Stripe + MyT Money
/confirmation         → Booking confirmed + itinerary
/admin                → Decap CMS (no-code content management)
```

---

## Services Offered

### 1. Accommodation
- Studio apartment (sleeps 2)
- 2-bedroom villa with pool & garden (sleeps 4-6)

### 2. Experiences
- Boat excursions (snorkelling, dolphin watching, île aux cerfs)
- Nature excursions (Black River Gorges, Chamarel, Pamplemousses)
- Local food discovery (street food tours, market visits, cooking classes)

### 3. Concierge Services
- Airport transfers
- Car/scooter rental
- Healthcare access (clinic referrals, pharmacy)
- Safety & security briefings
- SIM card & connectivity
- Event tickets & reservations
- Personal shopping

---

## Payment Architecture

### Stripe (International)
- Visa, Mastercard, Amex
- Apple Pay, Google Pay
- 3D Secure / SCA compliant

### MyT Money (Mauritius Local)
- MCB Juice integration
- MyT Money wallet
- Local bank transfer (Bank One, SBM, MCB, AfrAsia)
- Cash on arrival option (with deposit)

---

## Guest Registration & Booking Flow

1. **Register** → Name, email, phone, nationality, arrival date
2. **Browse** → Choose accommodation + add-on experiences
3. **Schedule** → Pick dates/times for each service
4. **Review** → Full itinerary summary with pricing
5. **Pay** → Stripe (international) or MyT Money (local)
6. **Confirm** → Email confirmation + WhatsApp notification
7. **Concierge** → Dedicated WhatsApp support throughout stay

---

## Environment Variables (Vercel)

```env
FORMSPREE_BOOKING_ID=your_id
STRIPE_PUBLISHABLE_KEY=pk_live_...
STRIPE_SECRET_KEY=sk_live_...        # Vercel env only
STRIPE_WEBHOOK_SECRET=whsec_...      # Vercel env only
NEXT_PUBLIC_WHATSAPP=23057000000
```

---

## Deployment

GitHub repo → Vercel auto-deploy on every push
Admin: yourdomain.com/admin (Decap CMS, GitHub OAuth)
Live: yourdomain.com

---

*Evalia Mauritius — v4.0 — April 2026*
