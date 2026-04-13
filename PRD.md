# Evalia Mauritius — Product Requirements Document (PRD)
## Version 4.0 — April 2026

---

## 1. Product Overview

**Product Name:** Evalia Mauritius
**Tagline:** "Your Mauritius, Perfectly Arranged"
**Type:** Tourist concierge & booking platform
**Primary Market:** International tourists visiting Mauritius
**Secondary Market:** Local residents seeking curated experiences

---

## 2. Problem Statement

Tourists arriving in Mauritius face fragmented experiences — accommodation, transport, excursions, food, and support services require separate bookings across multiple providers with no unified support. Safety concerns and language barriers add friction. Evalia solves this with a single trusted platform.

---

## 3. Target Users

| User Type | Description |
|-----------|-------------|
| International Tourist | Visiting Mauritius, wants everything arranged |
| Repeat Visitor | Knows Mauritius, wants curated local experiences |
| Family Traveller | Needs safety, reliability, family-friendly options |
| Business Traveller | Short stay, needs transfers, accommodation, support |

---

## 4. Services & Pricing (indicative)

### Accommodation
| Property | Capacity | Rate |
|----------|----------|------|
| Studio Apartment | 2 guests | €80/night |
| Villa with Pool & Garden (2-bed) | 4–6 guests | €180/night |

### Experiences
| Service | Duration | Price from |
|---------|----------|-----------|
| Boat Excursion | Full day | €65/person |
| Dolphin Watching | Half day | €45/person |
| Île aux Cerfs | Full day | €75/person |
| Black River Gorges | Full day | €55/person |
| Chamarel & 7 Colours | Half day | €40/person |
| Local Food Tour | 3 hours | €35/person |
| Cooking Class | 3 hours | €50/person |

### Concierge Services
| Service | Price |
|---------|-------|
| Airport Transfer (one way) | €30 |
| Car Rental (per day) | €45 |
| SIM Card Setup | €15 |
| Healthcare Referral | Free |
| Personal Shopping | €25/hour |

---

## 5. Functional Requirements

### FR-01: Guest Registration
- Name, email, phone, nationality
- Arrival & departure dates
- Party size & composition (adults, children)
- Special requirements (dietary, accessibility, medical)
- Email verification
- Guest profile stored for repeat bookings

### FR-02: Service Browsing
- All services displayed with photos, descriptions, pricing
- Filter by category (accommodation, excursions, concierge)
- Availability calendar per service
- Real-time availability check

### FR-03: Booking System
- Select one or multiple services in one session
- Date/time picker per service
- Guest count per service
- Special requests field
- Booking summary before payment

### FR-04: Scheduling
- Integrated calendar showing all booked services
- Itinerary view per guest (day-by-day)
- Automated conflict detection (same guest, overlapping times)
- Reminder emails 24h before each activity

### FR-05: Payment
- Stripe: Visa, Mastercard, Amex, Apple Pay, Google Pay
- MyT Money / MCB Juice (Mauritius local)
- Bank transfer option with manual confirmation
- Cash on arrival (with 30% deposit online)
- Multi-currency: EUR, GBP, USD, MUR
- Instant receipt by email

### FR-06: Confirmation & Communication
- Instant booking confirmation email
- WhatsApp notification with booking details
- PDF itinerary attached to confirmation email
- 24h reminder before each activity
- Cancellation/modification self-service

### FR-07: CMS (Admin)
- Add/edit/delete services and properties
- Upload photos directly (no external tool needed)
- Manage availability/blackout dates
- View all bookings and guest details
- No coding required

### FR-08: Safety & Trust
- Emergency contact information visible throughout
- 24/7 WhatsApp concierge number
- Healthcare referral network listed
- Guest safety briefing on confirmation email
- Verified reviews displayed on each service

---

## 6. Non-Functional Requirements

| Requirement | Target |
|-------------|--------|
| Page load speed | < 2 seconds |
| Mobile responsiveness | 100% — majority of users on mobile |
| Languages | English (primary), French (secondary) |
| Uptime | 99.9% (Vercel SLA) |
| Payment security | PCI DSS compliant (Stripe) |
| Data privacy | GDPR compliant |

---

## 7. Out of Scope (v1.0)

- Native mobile app (web-first)
- Multi-vendor marketplace
- Real-time chat (WhatsApp link used instead)
- Automated invoicing / accounting integration

---

## 8. Success Metrics

| Metric | Target (Month 3) |
|--------|-----------------|
| Bookings per month | 20+ |
| Avg booking value | €250+ |
| Guest registration | 50+ profiles |
| Page views/month | 500+ |
| WhatsApp enquiries converted | >40% |

---

*PRD Owner: Evalia Mauritius — April 2026*
