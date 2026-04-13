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

## Future Enhancements (Post-MVP)
- Native mobile app (React Native) with OS screen time API integration
- Push notifications for challenges and streaks
- NFC-based IRL Lock feature
- Partner portal for Luxembourg businesses
- Premium subscription tier
- Analytics dashboard for schools/educators
- Supabase Edge Functions for Moments calculation
- Real-time leaderboard updates via Supabase Realtime
