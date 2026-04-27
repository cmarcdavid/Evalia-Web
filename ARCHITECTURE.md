# Evalia Mauritius — System Architecture & Roadmap
*v5.12 · April 2026*

---

## Current Architecture (GitHub CMS)

```
Visitor → Vercel CDN → index.html
                      ↓ fetch()
              _data/*.json (GitHub raw)

Admin → admin.html → GitHub API → _data/*.json → Vercel rebuild → Live site
```

**What works well:**
- Zero server costs — Vercel free tier handles everything
- Admin edits go live in ~30 seconds
- No database to manage, no hosting bills
- Content is version-controlled automatically

**What it cannot do:**
- Real-time availability (GitHub files are not a lock system)
- Guest login and persistent sessions
- Payment processing (no server to receive webhooks)
- Abandoned booking automation (no background jobs)
- Scalable guest database (100+ guests = 100+ JSON files)

---

## Recommended Next Architecture: GitHub CMS + Supabase

Keep GitHub for **content** (properties, experiences, settings, media).  
Add Supabase for **transactional data** (guests, bookings, payments, availability).

```
Visitor → Vercel → index.html
              ↓                    ↓
        _data/*.json         Supabase REST API
        (CMS content)        (guests, bookings, availability)
              ↓                    ↓
        admin.html           Supabase Auth (login)
```

---

## Supabase Setup (Free Tier — €0/month to start)

### 1. Create Project
1. Go to [supabase.com](https://supabase.com) → New Project
2. Name: `evalia-mauritius`
3. Region: `eu-west-1` (Ireland — closest to Mauritius)
4. Note your **Project URL** and **anon key**

### 2. Database Tables

Run this SQL in the Supabase SQL Editor:

```sql
-- Guests table
CREATE TABLE guests (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  name TEXT NOT NULL,
  email TEXT UNIQUE NOT NULL,
  phone TEXT,
  nationality TEXT,
  party_size TEXT,
  arrival DATE,
  departure DATE,
  special_req TEXT,
  verified BOOLEAN DEFAULT FALSE,
  email_verified BOOLEAN DEFAULT FALSE,
  wa_verified BOOLEAN DEFAULT FALSE,
  payment_pref TEXT,
  quickbooks_id TEXT,
  stripe_id TEXT,
  notes TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Bookings table
CREATE TABLE bookings (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  ref TEXT UNIQUE NOT NULL,
  guest_id UUID REFERENCES guests(id),
  guest_email TEXT,
  guest_name TEXT,
  guest_phone TEXT,
  services TEXT[],
  checkin DATE,
  checkout DATE,
  guests_count INTEGER,
  status TEXT DEFAULT 'pending', -- pending | confirmed | cancelled | abandoned
  payment_method TEXT,
  estimated_total TEXT,
  notes TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Availability blocks
CREATE TABLE availability (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  date DATE NOT NULL,
  property TEXT NOT NULL, -- studio | villa | both | maintenance
  booked_by UUID REFERENCES bookings(id),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(date, property)
);

-- Enable Row Level Security
ALTER TABLE guests     ENABLE ROW LEVEL SECURITY;
ALTER TABLE bookings   ENABLE ROW LEVEL SECURITY;
ALTER TABLE availability ENABLE ROW LEVEL SECURITY;

-- Public can read availability (for booking form)
CREATE POLICY "Public read availability" ON availability FOR SELECT USING (true);

-- Public can insert guest registrations
CREATE POLICY "Public register" ON guests FOR INSERT WITH CHECK (true);

-- Only authenticated admin can read/write all
CREATE POLICY "Admin full access guests"   ON guests     FOR ALL USING (auth.role() = 'authenticated');
CREATE POLICY "Admin full access bookings" ON bookings   FOR ALL USING (auth.role() = 'authenticated');
CREATE POLICY "Admin write availability"   ON availability FOR ALL USING (auth.role() = 'authenticated');
```

### 3. Add Supabase to index.html

Replace the `GH_REPO` constant section with:

```html
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
<script>
  const SUPABASE_URL = 'https://YOUR_PROJECT.supabase.co';
  const SUPABASE_KEY = 'YOUR_ANON_KEY'; // public anon key — safe to expose
  const supabase = window.supabase.createClient(SUPABASE_URL, SUPABASE_KEY);
</script>
```

Replace `ghWriteGuest()` with:

```javascript
async function registerGuest(guestData) {
  const { data, error } = await supabase.from('guests').insert([guestData]).select();
  if (error) throw new Error(error.message);
  return data[0];
}
```

### 4. Availability Check in Booking Form

```javascript
async function checkAvailability(checkin, checkout, property) {
  const { data } = await supabase
    .from('availability')
    .select('date, property')
    .gte('date', checkin)
    .lte('date', checkout)
    .in('property', [property, 'both']);
  return data?.length === 0; // true = available
}
```

### 5. Admin Auth (Supabase Auth)

```javascript
// Admin login
const { data, error } = await supabase.auth.signInWithPassword({
  email: 'admin@evalia.mu',
  password: 'YOUR_ADMIN_PASSWORD'
});

// Replace GitHub token auth with Supabase session
const { data: { session } } = await supabase.auth.getSession();
```

---

## Migration Plan

### Phase 1 — Parallel (1 week, no downtime)
1. Create Supabase project and tables
2. Update `index.html` registration form to write to Supabase (keep GitHub fallback)
3. Update booking form to check Supabase availability
4. Update admin to read guests/bookings from Supabase

### Phase 2 — Auth (1 week)
1. Add Supabase Auth to admin (replace GitHub token flow)
2. Add guest login — email/password or magic link
3. Guest profile page showing their bookings and details
4. Email verification flow via Supabase Auth

### Phase 3 — Payments & Automation (2 weeks)
1. Stripe integration via Supabase Edge Functions
2. WhatsApp Business API webhook (receive and send messages)
3. Abandoned booking email/WA trigger (Supabase cron job)
4. Booking confirmation email (Supabase + Resend)

### Phase 4 — Advanced (ongoing)
1. Migrate from Formspree to Supabase entirely
2. Real-time admin dashboard (Supabase realtime)
3. QuickBooks / Zoho sync via Edge Functions
4. Analytics dashboard (bookings by month, revenue, occupancy rate)

---

## Cost Summary

| Tier | Monthly Cost | What you get |
|---|---|---|
| **Now (GitHub + Vercel free)** | €0 | CMS only, no guests/payments |
| **Supabase Free** | €0 | 500MB DB, 50K MAU, 2GB storage |
| **Supabase Pro** | ~€25 | 8GB DB, 100K MAU, 250GB storage, daily backups |
| **+Stripe** | 1.4% + €0.25/transaction | Card payments (EU rates) |
| **+Resend (email)** | €0 → €20 | 3K/mo free, then per send |
| **+WhatsApp Business** | ~€0.05/message | Meta pricing (conversation-based) |

**Realistic monthly cost at 50 bookings/month: ~€30–40 total.**

---

## What to Do Right Now (Today)

1. **Deploy v5.12** (`admin.html` + `index.html`) — fixes all bugs, adds calendar, chatbot, step 4 payment flow
2. **Create Supabase free account** at [supabase.com](https://supabase.com) — 5 minutes
3. **Run the SQL above** to create the tables
4. Tell me your Supabase URL and anon key and I'll wire it in v5.13

---

## Payment Flow Summary (Current → Target)

### Current (v5.12, no payment processor)
```
Guest fills booking form
       ↓
Formspree sends you email (ref: EVL-XXXXXX)
       ↓
You manually WhatsApp guest to confirm availability
       ↓
You send payment link manually (MyT Money, bank transfer, etc.)
       ↓
Guest pays, you confirm manually
```

### Target (v5.13+, Stripe + Supabase)
```
Guest fills booking form
       ↓
Supabase checks availability in real-time (blocks if taken)
       ↓
Booking record created in Supabase (status: pending)
       ↓
Supabase Edge Function sends WhatsApp to you (new booking alert)
       ↓
You click "Confirm" in admin → status → confirmed
       ↓
Stripe payment link auto-sent to guest via WhatsApp + email
       ↓
Guest pays → Stripe webhook → Supabase updates status → confirmed
       ↓
Auto-confirmation email + itinerary sent to guest
```

### What you need to set up Stripe:
1. [stripe.com](https://stripe.com) business account (MU entity or personal)
2. Verify your business (ID + bank details, ~2 days)
3. Create a Payment Link product per property/service
4. Tell me and I'll wire the links into the booking confirmation flow

---

*Document generated by Evalia Admin System v5.12*  
*Jira: WEBSITE-176 · Repo: cmarcdavid/evalia-web*
