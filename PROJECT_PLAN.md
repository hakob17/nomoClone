# NomoLux - Digital Wellness App for Luxembourg

> A NOMO-inspired platform adapted for the Luxembourg market. Gamifies screen time reduction through a "Moments" points system, local rewards, leaderboards, and social challenges.

---

## Overview

| | |
|---|---|
| **Concept** | Encourage reduced social media usage through gamification and real rewards |
| **Target Market** | Luxembourg - multilingual (EN/FR/DE/LU), local partners |
| **Platform** | Web app (Next.js) - marketing site + authenticated dashboard |
| **Stack** | Next.js 14 (App Router) + Tailwind CSS + Supabase + next-intl |
| **Estimated Files** | ~100 |

---

## Core Features

### 1. Moments System
Users earn "Moments" (points) by hitting scroll-reduction milestones. These points are the platform's primary currency for unlocking rewards.

### 2. Luxembourg Rewards Catalog
Redeem Moments for local perks:
- **Events**: Philharmonie concerts, Mudam exhibitions, national holiday fireworks
- **Restaurants**: Knauf, Auchan vouchers, local bistro discounts
- **Shopping**: Ville Haute boutiques, Belle Etoile gift cards
- **Cash-back**: Direct cashback rewards
- **Merchandise**: Branded NomoLux gear

### 3. Social Competition
- **Leaderboards**: Daily, weekly, monthly, all-time rankings
- **Friend Challenges**: Challenge friends to reduce screen time
- **Friend System**: Add friends, see their progress

### 4. IRL Lock ("Fist Bump")
Two users can physically interact (NFC/proximity) to lock social media for 30-60 min, earning bonus Moments. (Web MVP: manual activation with friend code)

### 5. Multilingual Support
Full i18n in 4 languages: English, French, German, Luxembourgish

---

## Pages & User Flows

### Marketing Website (Public)

| Page | Description |
|---|---|
| **Landing** (`/`) | Hero section, features showcase, how it works, perks preview, stats, CTA |
| **Parents** (`/parents`) | Resources for parents on digital wellness |
| **Educators** (`/educators`) | Resources for schools and educators |
| **Privacy** (`/privacy`) | Privacy policy - no individual screen time data stored/shared |

### Authentication

| Page | Description |
|---|---|
| **Sign Up** (`/signup`) | Email/password or Google/Apple OAuth |
| **Log In** (`/login`) | Email/password or OAuth |
| **Forgot Password** (`/forgot-password`) | Password reset flow |

### Dashboard (Authenticated)

| Page | Description |
|---|---|
| **Dashboard** (`/dashboard`) | Screen time chart, Moments earned, streak, daily progress, quick actions |
| **Rewards** (`/rewards`) | Browse & filter Luxembourg rewards by category, redeem with Moments |
| **Reward Detail** (`/rewards/[id]`) | Full reward info, partner details, redeem button |
| **Leaderboard** (`/leaderboard`) | Rankings with tabs (daily/weekly/monthly/all-time), friends filter |
| **Challenges** (`/challenges`) | Active challenges, create new, track progress |
| **Settings** (`/settings`) | Profile, language, theme, account management |

---

## Tech Stack

### Frontend
| Technology | Purpose |
|---|---|
| **Next.js 14** (App Router) | Full-stack React framework |
| **TypeScript** | Type safety |
| **Tailwind CSS** | Utility-first styling |
| **Radix UI** | Accessible UI primitives (dialog, dropdown, tabs, etc.) |
| **Framer Motion** | Animations and transitions |
| **Recharts** | Dashboard charts (screen time visualization) |
| **Lucide React** | Icon library |
| **next-intl** | Internationalization (4 languages) |
| **next-themes** | Dark/light mode |

### Backend
| Technology | Purpose |
|---|---|
| **Supabase** | Authentication + PostgreSQL database + Row Level Security |
| **@supabase/ssr** | Server-side session management with cookies |
| **Zod** | Schema validation for forms and API inputs |

---

## Database Schema

### Tables

```
profiles
  - id (uuid, FK -> auth.users)
  - username (text, unique)
  - display_name (text)
  - avatar_url (text)
  - locale (en/fr/de/lu)
  - total_moments (int)
  - current_streak (int)
  - longest_streak (int)
  - last_active_date (date)

daily_logs
  - id (uuid)
  - user_id (FK -> profiles)
  - log_date (date, unique per user)
  - screen_time_minutes (int)
  - social_media_minutes (int)
  - moments_earned (int)
  - goal_minutes (int)

milestones
  - id (uuid)
  - name (text)
  - description_key (text, i18n key)
  - moments_value (int)
  - icon (text)

user_milestones
  - user_id (FK -> profiles)
  - milestone_id (FK -> milestones)
  - earned_at (timestamptz)

rewards
  - id (uuid)
  - title_{en,fr,de,lu} (text)
  - description_{en,fr,de,lu} (text)
  - category (events/restaurants/shopping/cashback/merchandise)
  - moments_cost (int)
  - image_url (text)
  - partner_name (text)
  - partner_logo_url (text)
  - location (text)
  - total_available (int)
  - is_active (boolean)
  - expires_at (timestamptz)

redemptions
  - id (uuid)
  - user_id (FK -> profiles)
  - reward_id (FK -> rewards)
  - moments_spent (int)
  - redemption_code (text, unique)
  - status (active/used/expired)

leaderboard
  - user_id (FK -> profiles, unique)
  - period (daily/weekly/monthly/alltime)
  - moments_total (int)
  - rank (int)

friendships
  - requester_id (FK -> profiles)
  - addressee_id (FK -> profiles)
  - status (pending/accepted/blocked)

challenges
  - id (uuid)
  - creator_id (FK -> profiles)
  - title (text)
  - challenge_type (reduction/streak/moments)
  - target_value (int)
  - duration_days (int)
  - starts_at, ends_at (timestamptz)

challenge_participants
  - challenge_id (FK -> challenges)
  - user_id (FK -> profiles)
  - current_progress (int)
```

---

## Project Structure

```
nomoClone/
├── messages/
│   ├── en.json              # English translations
│   ├── fr.json              # French translations
│   ├── de.json              # German translations
│   └── lu.json              # Luxembourgish translations
├── public/
│   └── images/
├── src/
│   ├── middleware.ts         # Locale routing + Supabase session
│   ├── i18n/
│   │   ├── routing.ts       # Locale config
│   │   └── request.ts       # Server-side i18n
│   ├── app/
│   │   ├── layout.tsx       # Root layout
│   │   └── [locale]/
│   │       ├── layout.tsx   # Locale provider
│   │       ├── page.tsx     # Landing page
│   │       ├── (marketing)/ # Nav + footer layout
│   │       │   ├── parents/
│   │       │   ├── educators/
│   │       │   └── privacy/
│   │       ├── (auth)/      # Centered card layout
│   │       │   ├── login/
│   │       │   ├── signup/
│   │       │   ├── callback/
│   │       │   └── forgot-password/
│   │       └── (dashboard)/ # Sidebar + topbar layout
│   │           ├── dashboard/
│   │           ├── rewards/
│   │           ├── leaderboard/
│   │           ├── challenges/
│   │           └── settings/
│   ├── components/
│   │   ├── ui/              # Button, Card, Badge, Input, etc.
│   │   ├── marketing/       # Hero, Features, HowItWorks, etc.
│   │   ├── layout/          # Nav, Footer, Sidebar, Topbar
│   │   ├── dashboard/       # Charts, Counters, Progress
│   │   ├── rewards/         # RewardCard, RewardGrid, Redeem
│   │   ├── leaderboard/     # Table, RankBadge
│   │   ├── challenges/      # ChallengeCard, CreateForm
│   │   └── auth/            # LoginForm, SignupForm
│   ├── lib/
│   │   ├── supabase/        # Client, Server, Middleware helpers
│   │   ├── utils.ts         # cn() helper, formatters
│   │   ├── constants.ts
│   │   └── validations.ts   # Zod schemas
│   ├── hooks/               # useUser, useMoments, useRewards, useLeaderboard
│   └── types/               # Database types, domain types
└── supabase/
    └── migrations/          # 6 SQL migration files
```

---

## Design System

### Color Palette (Luxembourg-inspired)

| Token | Hex | Usage |
|---|---|---|
| `nomo-primary` | `#EF4135` | Primary actions, highlights (Luxembourg red) |
| `nomo-accent` | `#00A1DE` | Secondary actions, links (Luxembourg blue) |
| `nomo-dark` | `#0F172A` | Dark mode background |
| `nomo-light` | `#F8FAFC` | Light mode background |

### Typography
- **Headings**: Bold, modern sans-serif (Inter or similar)
- **Body**: Clean, readable sans-serif
- **Hero text**: Extra-bold, large scale

### Design Principles
- Mobile-first responsive design
- Bold, youthful aesthetic
- Dark/light mode support
- Smooth entrance animations (Framer Motion)
- Accessible (Radix UI primitives)

---

## Authentication Flow

```
User visits /login or /signup
    |
    ├── Email/Password
    │     1. signUp / signInWithPassword
    │     2. Email confirmation (signup)
    │     3. Session set via cookies
    │     4. Auto-create profile (DB trigger)
    │     5. Redirect to /dashboard
    |
    └── Google/Apple OAuth
          1. signInWithOAuth -> redirect to provider
          2. Callback at /callback/route.ts
          3. Exchange code for session
          4. Auto-create profile (DB trigger)
          5. Redirect to /dashboard

Middleware protects /dashboard/* routes
```

---

## Implementation Phases

| Phase | Scope | Key Deliverables |
|---|---|---|
| **1** | Scaffold & Config | Next.js app, Tailwind theme, i18n setup, Supabase clients |
| **2** | Database | All migrations, RLS policies, seed data |
| **3** | UI Primitives & Layouts | Reusable components, marketing/auth/dashboard shells |
| **4** | Authentication | Login, signup, OAuth, protected routes |
| **5** | Marketing Landing | Hero, features, how-it-works, perks, CTA |
| **6** | Dashboard Core | Charts, moments, streaks, daily logging |
| **7** | Rewards, Leaderboard, Challenges | Browse/redeem rewards, rankings, social features |
| **8** | Polish & Deploy | Full i18n, responsive audit, animations, Vercel deploy |

---

## Key Architecture Decisions

| Decision | Rationale |
|---|---|
| Route groups for layout isolation | `(marketing)`, `(auth)`, `(dashboard)` get separate shells without URL impact |
| next-intl over alternatives | Best App Router support, clean API |
| Radix UI + Tailwind over component library | Full control, small bundle, accessible |
| Manual screen time logging | Web app limitation - no OS API access |
| Per-locale DB columns for rewards | Simple, no joins, fast locale-aware queries |
| Precomputed leaderboard table | Avoids expensive real-time aggregation |

---

## Environment Variables

```env
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key
```

---

## Business Model & Monetization

### Revenue Streams

#### 1. Freemium Subscription (Primary Revenue)

| Tier | Price | Features |
|---|---|---|
| **Free** | 0 | Basic screen time tracking, 5 daily log entries, limited rewards catalog, public leaderboard |
| **NomoLux Plus** | 4.99/month or 39.99/year | Unlimited tracking, full rewards catalog, friend challenges, detailed analytics, priority reward access |
| **NomoLux Family** | 9.99/month or 79.99/year | Up to 5 family members, parental dashboard, family challenges, shared rewards wallet |

**Conversion assumption**: 5-8% free-to-paid conversion rate (industry average for wellness apps is 4-10%)

#### 2. Partner Commissions (Luxembourg Local Business Network)

| Partner Type | Commission Model | Est. Revenue Per Transaction |
|---|---|---|
| Restaurants | 10-15% per voucher redeemed | 3-8 |
| Retail/Shopping | 8-12% per gift card sold | 2-10 |
| Events/Culture | 5-10% per ticket sold | 2-15 |
| Cash-back partners | 2-5% affiliate commission | 0.50-3 |

**Target**: 20-50 local Luxembourg partners in Year 1, growing to 100+ by Year 2.
Partner onboarding fee: 200 one-time setup + revenue share.

#### 3. Institutional/B2B Licensing

| Client | Pricing | Description |
|---|---|---|
| **Schools** | 500-2,000/year per school | Educator dashboard, class leaderboards, student wellness reports |
| **Employers** | 2-5/employee/month | Corporate wellness program integration, team challenges |
| **Municipalities** | 5,000-15,000/year | City-wide digital wellness initiatives, public leaderboards |

Luxembourg has ~180 schools (secondary/lycees) and ~4,000 employers. Even 5% penetration = meaningful B2B revenue.

#### 4. Sponsored Challenges & Brand Partnerships

- Brands sponsor challenges (e.g., "POST Luxembourg Digital Detox Week")
- Sponsored rewards slots in the catalog (featured placement)
- Co-branded campaigns with telecom providers (POST, Tango, Orange Luxembourg)
- **Pricing**: 1,000-10,000 per campaign depending on reach

#### 5. Data Insights (Anonymized & Aggregated)

- Sell anonymized, aggregated digital wellness trend reports to:
  - Government health agencies (Ministry of Health)
  - Research institutions (University of Luxembourg)
  - NGOs focused on digital wellness
- **No individual data shared** - only aggregate trends
- **Pricing**: 5,000-20,000 per annual report/dataset

---

## Financial Projections & Break-Even Analysis

### Cost Structure (Monthly)

| Cost Item | Month 1-6 | Month 7-12 | Year 2 |
|---|---|---|---|
| Supabase (DB + Auth) | 25 | 75 | 150 |
| Vercel hosting | 20 | 50 | 100 |
| Domain + SSL | 5 | 5 | 5 |
| Email service (Resend) | 0 | 20 | 50 |
| Marketing/Ads | 500 | 1,500 | 3,000 |
| Partner acquisition | 200 | 500 | 500 |
| Customer support | 0 | 500 | 1,500 |
| Development/Maintenance | 2,000 | 2,000 | 3,000 |
| Legal/Compliance (GDPR) | 200 | 100 | 100 |
| **Total Monthly Costs** | **2,950** | **4,750** | **8,405** |

### Revenue Model by User Count

**Assumptions:**
- Luxembourg population: ~660,000 (target demographic 12-35: ~180,000)
- Free-to-paid conversion: 6%
- Average partner commission: 5 per redemption
- Average redemptions per paying user: 1.5/month
- B2B revenue starts Month 9

#### Scenario: User Growth & Revenue

| Users (Total) | Paying Users (6%) | Subscription Rev/mo | Partner Commissions/mo | B2B Rev/mo | Total Rev/mo | Monthly Costs | Net/mo |
|---|---|---|---|---|---|---|---|
| **500** | 30 | 150 | 75 | 0 | 225 | 2,950 | -2,725 |
| **1,000** | 60 | 300 | 150 | 0 | 450 | 3,200 | -2,750 |
| **2,500** | 150 | 750 | 563 | 0 | 1,313 | 3,800 | -2,488 |
| **5,000** | 300 | 1,500 | 1,125 | 1,000 | 3,625 | 4,750 | -1,125 |
| **8,000** | 480 | 2,400 | 1,800 | 2,500 | 6,700 | 5,500 | **+1,200** |
| **10,000** | 600 | 3,000 | 2,250 | 3,500 | 8,750 | 6,000 | **+2,750** |
| **15,000** | 900 | 4,500 | 3,375 | 5,000 | 12,875 | 7,000 | **+5,875** |
| **25,000** | 1,500 | 7,500 | 5,625 | 8,000 | 21,125 | 8,405 | **+12,720** |
| **50,000** | 3,000 | 15,000 | 11,250 | 12,000 | 38,250 | 12,000 | **+26,250** |

### Break-Even Analysis

```
BREAK-EVEN POINT: ~7,000-8,000 total users
                   (~420-480 paying subscribers)

Timeline to break-even (realistic growth scenario):
  Month 1-3:   Beta launch, 200-500 users (word of mouth, schools pilot)
  Month 4-6:   Marketing push, 500-2,000 users
  Month 7-9:   Partner network live, 2,000-4,000 users
  Month 10-12: B2B contracts start, 4,000-6,000 users
  Month 13-16: BREAK-EVEN at ~7,500 users
  Month 18:    10,000 users, profitable at ~2,750/month
  Month 24:    20,000-25,000 users, ~10,000/month profit

TOTAL INVESTMENT TO BREAK-EVEN: ~55,000-65,000
```

### Revenue Composition at Scale (25,000 users)

```
Subscriptions:     7,500/mo   (36%)  ████████████
Partner Revenue:   5,625/mo   (27%)  █████████
B2B Licensing:     8,000/mo   (38%)  ████████████▌
Sponsored:         variable          (bonus)
                   ─────────
Total:            21,125/mo   (100%)

Annual Revenue:   ~253,000
Annual Costs:     ~101,000
Annual Profit:    ~152,000
```

### Luxembourg Market Opportunity

| Metric | Value |
|---|---|
| Total population | ~660,000 |
| Target demographic (12-35) | ~180,000 |
| Realistic addressable market (digital natives with smartphones) | ~150,000 |
| Conservative penetration (Year 1) | 3-5% = 4,500-7,500 users |
| Moderate penetration (Year 2) | 8-15% = 12,000-22,500 users |
| Ambitious penetration (Year 3) | 15-25% = 22,500-37,500 users |

Luxembourg advantages:
- **Small, dense market**: Easy to saturate with targeted marketing
- **High smartphone penetration**: 95%+ in target demographic
- **Wealthy population**: High willingness to pay for premium services
- **Multilingual**: App supports all 4 official/common languages
- **Strong school system**: Direct channel to youth via ~180 secondary schools
- **Local pride**: "Made in Luxembourg" resonates with local market

### Key Metrics to Track (KPIs)

| Metric | Target (Month 6) | Target (Month 12) | Target (Month 24) |
|---|---|---|---|
| Total registered users | 2,000 | 6,000 | 25,000 |
| Monthly Active Users (MAU) | 1,200 | 4,000 | 18,000 |
| Paid subscribers | 120 | 360 | 1,500 |
| Free-to-paid conversion | 6% | 6% | 6% |
| Monthly churn rate | <8% | <5% | <4% |
| Partner businesses | 10 | 30 | 80 |
| B2B contracts | 0 | 5 | 20 |
| Avg. Moments earned/user/day | 50 | 75 | 100 |
| Avg. daily screen time reduction | 15 min | 25 min | 35 min |
| NPS Score | 30+ | 40+ | 50+ |

### Funding Requirements

| Phase | Amount | Purpose |
|---|---|---|
| **Pre-seed (Month 0-3)** | 15,000 | MVP development, legal setup, initial marketing |
| **Seed (Month 4-9)** | 30,000 | Marketing campaigns, partner acquisition, first hire |
| **Growth (Month 10-18)** | 20,000 | Scale marketing, B2B sales, native app development |
| **Total to profitability** | **65,000** | Covers runway until ~Month 16 break-even |

Luxembourg-specific funding sources:
- **Fit4Start** accelerator (up to 150,000 + coaching)
- **Luxembourg National Research Fund (FNR)** grants
- **Luxinnovation** support programs
- **Digital Luxembourg** initiative alignment
- **Banque de Luxembourg** startup programs

---

## Future Enhancements (Post-MVP)
- Native mobile app (React Native) with OS screen time API integration
- Push notifications for challenges and streaks
- NFC-based IRL Lock feature
- Partner portal for Luxembourg businesses
- Premium subscription tier
- Analytics dashboard for schools/educators
- Supabase Edge Functions for Moments calculation
- Real-time leaderboard updates via Supabase Realtime
