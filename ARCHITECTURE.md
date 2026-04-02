# Evalia — Website Architecture & Documentation

## Overview
Evalia is a curated vacation rental platform with a stylish property showcase, integrated booking system, and secure payment module.

---

## Tech Stack

### Frontend (Delivered)
- Pure HTML5 / CSS3 / Vanilla JavaScript (zero dependencies)
- Google Fonts: Cormorant Garamond (display) + DM Sans (body)
- Fully responsive: mobile, tablet, desktop

### Recommended Production Stack
| Layer | Technology |
|-------|-----------|
| Frontend Framework | React 18 + Vite |
| Styling | Tailwind CSS + custom design tokens |
| Backend API | Node.js + Express OR Next.js (API routes) |
| Database | PostgreSQL (properties, bookings, users) |
| Auth | Clerk or NextAuth.js |
| Payments | Stripe (cards, wallets, bank) |
| Media Storage | Cloudinary (images + videos) |
| Email | Resend or SendGrid |
| Hosting | Vercel (frontend) + Railway (backend) |

---

## Site Structure

```
/                        → Hero + Search + Stats
/properties              → Property grid with filters
/properties/:slug        → Individual property detail + photo gallery + video tour
/booking                 → Booking wizard (property → dates → guests → confirm)
/payment                 → Stripe checkout (card / Apple Pay / Google Pay / bank)
/confirmation            → Booking confirmation + PDF invoice
/admin                   → Property management dashboard (CMS)
/admin/properties/new    → Add/edit property with media upload
/admin/bookings          → Booking calendar + guest management
```

---

## Key Features

### 1. Property Showcase
- Masonry/grid layout with lazy-loaded images
- Video tour player (Cloudinary HLS stream)
- Amenities filter (villa, apartment, chalet, beachfront)
- Property detail modal with full gallery lightbox
- Favourite / save to wishlist

### 2. Booking System
- Real-time availability calendar (react-day-picker)
- Dynamic price calculator (base rate × nights + cleaning fee)
- Guest count selector with max-capacity validation
- Booking request → confirmation email (SendGrid template)
- Admin approval workflow OR instant book toggle

### 3. Payment Module
- Stripe Payment Intents (PCI-compliant, no card data on server)
- Supported methods: Visa, Mastercard, Amex, Apple Pay, Google Pay, SEPA bank transfer
- 3D Secure (SCA compliant for EU)
- Automatic receipt + PDF invoice (PDFKit)
- Refund workflow via Stripe dashboard
- Multi-currency: EUR, GBP, USD, XAF

### 4. Admin CMS
- Add / edit / delete properties
- Upload images & videos (drag & drop → Cloudinary)
- Set nightly rates, minimum stay, availability
- View all bookings with status (pending / confirmed / checked-in / completed)
- Revenue analytics dashboard

---

## Database Schema (PostgreSQL)

```sql
-- Properties
CREATE TABLE properties (
  id          SERIAL PRIMARY KEY,
  slug        TEXT UNIQUE NOT NULL,
  name        TEXT NOT NULL,
  location    TEXT,
  type        TEXT,          -- villa | apartment | chalet | beach
  description TEXT,
  beds        INT,
  baths       INT,
  max_guests  INT,
  price_night NUMERIC(10,2),
  cleaning_fee NUMERIC(10,2) DEFAULT 85,
  available   BOOLEAN DEFAULT TRUE,
  created_at  TIMESTAMPTZ DEFAULT NOW()
);

-- Media
CREATE TABLE media (
  id          SERIAL PRIMARY KEY,
  property_id INT REFERENCES properties(id),
  url         TEXT NOT NULL,
  type        TEXT,          -- image | video
  order_index INT DEFAULT 0
);

-- Bookings
CREATE TABLE bookings (
  id          SERIAL PRIMARY KEY,
  property_id INT REFERENCES properties(id),
  guest_name  TEXT,
  guest_email TEXT,
  check_in    DATE,
  check_out   DATE,
  guests      INT,
  total       NUMERIC(10,2),
  status      TEXT DEFAULT 'pending',  -- pending | confirmed | cancelled | completed
  stripe_id   TEXT,
  created_at  TIMESTAMPTZ DEFAULT NOW()
);

-- Blocked Dates
CREATE TABLE blocked_dates (
  id          SERIAL PRIMARY KEY,
  property_id INT REFERENCES properties(id),
  date        DATE NOT NULL
);
```

---

## Environment Variables

```env
# Stripe
STRIPE_SECRET_KEY=sk_live_...
STRIPE_PUBLISHABLE_KEY=pk_live_...
STRIPE_WEBHOOK_SECRET=whsec_...

# Database
DATABASE_URL=postgresql://user:pass@host:5432/evalia

# Cloudinary
CLOUDINARY_CLOUD_NAME=evalia
CLOUDINARY_API_KEY=...
CLOUDINARY_API_SECRET=...

# Email
SENDGRID_API_KEY=SG.xxx
FROM_EMAIL=hello@evalia.co

# Auth
NEXTAUTH_SECRET=...
NEXTAUTH_URL=https://evalia.co
```

---

## Deployment (Vercel)

1. Push code to GitHub
2. Connect repo to Vercel
3. Set environment variables in Vercel dashboard
4. Set up PostgreSQL via Vercel Postgres or Neon
5. Configure Stripe webhook endpoint: `https://evalia.co/api/webhooks/stripe`
6. Custom domain: point DNS A record to Vercel

---

## SEO & Performance

- Next.js ISR for property pages (revalidate every 60s)
- Open Graph tags per property (og:image from Cloudinary)
- Sitemap auto-generated
- Core Web Vitals target: LCP < 2.5s, FID < 100ms, CLS < 0.1

---

## Social Media Integration

- Instagram feed widget (Instagram Basic Display API)
- Facebook Page plugin in footer
- Share buttons on property pages
- Auto-post to Instagram/Facebook on new property listing (via Zapier or Make)

---

*Generated for Evalia — @evalia241 · April 2025*
