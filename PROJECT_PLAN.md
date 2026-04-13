# NomoLux - Digital Wellness App for Luxembourg

> A NOMO-inspired platform adapted for the Luxembourg market. Gamifies screen time reduction through a "Moments" points system, local rewards, leaderboards, and social challenges.

---

## Overview

| | |
|---|---|
| **Concept** | Encourage reduced social media usage through gamification and real rewards |
| **Target Market** | Luxembourg - multilingual (EN/FR/DE/LU), local partners |
| **Platform** | Native mobile app (iOS + Android) + Marketing website + Admin dashboard |
| **Mobile Stack** | React Native (Expo) + TypeScript + NativeWind |
| **Web Stack** | Next.js 14 (App Router) + Tailwind CSS (marketing site + admin) |
| **Backend** | Supabase (Auth + PostgreSQL + Edge Functions + Realtime) |
| **Go-to-Market** | Schools-first B2B, then consumer growth |

---

## Why Native App First

| Web App (previous plan) | Native App (current plan) |
|---|---|
| Manual screen time logging - terrible UX | Automatic screen time tracking via OS APIs |
| No push notifications | Push notifications for streaks, challenges, rewards |
| No background processing | Background usage monitoring |
| No NFC for IRL Lock | Real NFC fist-bump feature |
| Can't appear in App Store/Play Store | App Store presence = credibility + discovery |
| Poor retention (users won't manually log) | Passive tracking = effortless = high retention |
| No widget support | Home screen widgets showing daily progress |

**Bottom line**: Without real screen time data, the core product is broken. Native app is not a "nice to have" - it's the product.

---

## Core Features

### 1. Automatic Screen Time Tracking
- **iOS**: ScreenTime API (DeviceActivityFramework / FamilyControls)
- **Android**: UsageStatsManager API
- Tracks total screen time, per-app breakdown, social media usage
- All data stays on-device; only aggregated daily totals sent to backend
- Users grant permission once, tracking is passive from then on

### 2. Moments System
Users earn "Moments" (points) automatically based on:
- Staying under daily screen time goal (+10-50 Moments)
- Reducing social media vs. 7-day rolling average (+5-25 Moments)
- Maintaining streaks (+streak_days * 2 Moments)
- Completing challenges (+bonus Moments)
- IRL Lock sessions (+15 Moments per session)

### 3. Luxembourg Rewards Catalog
Redeem Moments for local perks:
- **Events**: Philharmonie concerts, Mudam exhibitions, national holiday events
- **Restaurants**: Knauf, Auchan vouchers, local bistro discounts
- **Shopping**: Ville Haute boutiques, Belle Etoile gift cards
- **Cash-back**: Direct cashback rewards
- **Merchandise**: Branded NomoLux gear

### 4. Social Competition
- **Leaderboards**: Daily, weekly, monthly, all-time rankings
- **Friend Challenges**: Challenge friends to reduce screen time
- **Friend System**: Add friends via username, contacts, or QR code
- **Class/Team boards**: For schools and workplaces

### 5. IRL Lock ("Fist Bump")
Two users physically tap phones together (NFC) to lock social media for 30-60 min:
- Both users earn bonus Moments
- Creates a social accountability moment
- Fallback: QR code scan if NFC unavailable

### 6. Smart Notifications
- Morning: "Your goal today: stay under 2h screen time"
- Midday: "You've used 45min so far - on track for 20 Moments!"
- Achievement: "You just earned the 7-Day Streak badge!"
- Social: "Alex challenged you to a Screen-Free Weekend"
- Gentle nudge: "You've been on Instagram for 30min. Take a break?"

### 7. Home Screen Widgets
- **iOS Widget**: Daily Moments earned, screen time remaining, streak count
- **Android Widget**: Same data, Material You adaptive styling

### 8. Multilingual Support
Full i18n in 4 languages: English, French, German, Luxembourgish

---

## App Screens & User Flows

### Onboarding (First Launch)

| Screen | Description |
|---|---|
| **Welcome** | 3-slide carousel: concept, how Moments work, local rewards |
| **Language Select** | Pick primary language (EN/FR/DE/LU) |
| **Sign Up** | Email/password, Google, or Apple Sign-In |
| **Permission Grant** | Request screen time access (iOS: FamilyControls, Android: UsageStats) |
| **Set Daily Goal** | Slider to set target screen time (default: 2h) |
| **Notification Opt-in** | Enable push notifications |
| **Ready** | "You're all set! Start earning Moments" |

### Main App (Tab Navigation)

| Tab | Screen | Description |
|---|---|---|
| **Home** | Dashboard | Today's screen time (live), Moments earned, streak, progress ring, quick actions |
| **Rewards** | Rewards Catalog | Browse & filter Luxembourg rewards by category, search, redeem |
| | Reward Detail | Full info, partner details, map location, redeem button |
| | My Rewards | Redeemed rewards with QR codes/voucher codes |
| **Social** | Leaderboard | Friends / Global / Class tabs, weekly/monthly/all-time |
| | Challenges | Active challenges, create new, invite friends |
| | Challenge Detail | Progress bars for all participants, chat |
| | Friends | Friend list, add via username/QR/contacts, pending requests |
| **Profile** | Profile | Stats overview, badges earned, Moments history |
| | Settings | Language, notifications, daily goal, theme, privacy, account |
| | Screen Time Detail | Per-app breakdown, trends over 7/30 days |

### Marketing Website (Next.js - separate project)

| Page | Description |
|---|---|
| **Landing** (`/`) | Hero, features showcase, how it works, perks preview, app store buttons, stats |
| **Parents** (`/parents`) | How NomoLux protects kids, family plan info, privacy details |
| **Educators** (`/educators`) | School program info, B2B pricing, pilot signup form |
| **Privacy** (`/privacy`) | GDPR-compliant privacy policy, data handling details |
| **Blog** (`/blog`) | Digital wellness tips, Luxembourg partner announcements |

---

## Tech Stack

### Mobile App
| Technology | Purpose |
|---|---|
| **React Native** (Expo SDK 52+) | Cross-platform iOS + Android |
| **TypeScript** | Type safety |
| **Expo Router** | File-based navigation (tabs, stacks, modals) |
| **NativeWind v4** | Tailwind CSS for React Native |
| **React Native Reanimated** | Smooth 60fps animations |
| **React Native Gesture Handler** | Swipe, pinch, tap gestures |
| **expo-notifications** | Push notifications |
| **expo-localization** + **i18n-js** | Multilingual support |
| **expo-screen-time** (custom module) | Screen time API bridge |
| **react-native-nfc-manager** | NFC for IRL Lock |
| **expo-widgets** | Home screen widgets |
| **Victory Native** | Charts (screen time visualization) |
| **Zustand** | Lightweight state management |
| **React Query (TanStack)** | Server state, caching, sync |

### Backend
| Technology | Purpose |
|---|---|
| **Supabase** | Auth + PostgreSQL + Row Level Security + Realtime |
| **Supabase Edge Functions** | Moments calculation, leaderboard refresh, reward redemption |
| **Supabase Realtime** | Live leaderboard updates, challenge progress sync |
| **Supabase Storage** | Avatar uploads, reward images |
| **Zod** | Schema validation |

### Marketing Website
| Technology | Purpose |
|---|---|
| **Next.js 14** (App Router) | Marketing site + admin dashboard |
| **Tailwind CSS** | Styling |
| **Framer Motion** | Landing page animations |
| **next-intl** | Multilingual marketing pages |

### Native Modules (Custom Expo Modules)

#### Screen Time Bridge (Critical)
```
iOS:  DeviceActivityFramework + FamilyControls (iOS 16+)
      - Request authorization via AuthorizationCenter
      - Monitor DeviceActivitySchedule
      - Read DeviceActivityReport for usage data
      - Shield apps during IRL Lock sessions

Android: UsageStatsManager (API 22+)
         - Request PACKAGE_USAGE_STATS permission
         - Query usage events per time interval
         - Aggregate by app category (social, entertainment, etc.)
```

This is the most complex part of the build. Both platforms have different APIs and permission models. Plan for 2-3 weeks of native module development.

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
  - daily_goal_minutes (int, default 120)
  - total_moments (int)
  - current_streak (int)
  - longest_streak (int)
  - last_active_date (date)
  - push_token (text)                    -- expo push token
  - device_platform (ios/android)
  - onboarding_complete (boolean)
  - subscription_tier (free/plus/family)
  - subscription_expires_at (timestamptz)
  - created_at, updated_at (timestamptz)

daily_logs
  - id (uuid)
  - user_id (FK -> profiles)
  - log_date (date, unique per user)
  - total_screen_minutes (int)           -- auto-tracked from OS
  - social_media_minutes (int)           -- auto-tracked
  - goal_minutes (int)                   -- snapshot of user's goal that day
  - moments_earned (int)
  - under_goal (boolean)                 -- computed: total < goal
  - app_breakdown (jsonb)                -- {instagram: 25, tiktok: 40, ...}
  - created_at (timestamptz)

milestones
  - id (uuid)
  - name (text)
  - description_key (text, i18n key)
  - moments_value (int)
  - icon (text)
  - requirement_type (streak/reduction/total_moments/challenge)
  - requirement_value (int)

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
  - location_name (text)
  - location_lat (float)
  - location_lng (float)
  - total_available (int)
  - total_redeemed (int, default 0)
  - is_active (boolean)
  - is_featured (boolean)
  - expires_at (timestamptz)

redemptions
  - id (uuid)
  - user_id (FK -> profiles)
  - reward_id (FK -> rewards)
  - moments_spent (int)
  - redemption_code (text, unique)
  - qr_data (text)                       -- for QR code display in app
  - status (active/used/expired)
  - redeemed_at (timestamptz)
  - used_at (timestamptz)

leaderboard
  - user_id (FK -> profiles, unique per period)
  - period (daily/weekly/monthly/alltime)
  - moments_total (int)
  - rank (int)
  - updated_at (timestamptz)

friendships
  - requester_id (FK -> profiles)
  - addressee_id (FK -> profiles)
  - status (pending/accepted/blocked)
  - created_at (timestamptz)

challenges
  - id (uuid)
  - creator_id (FK -> profiles)
  - title (text)
  - description (text)
  - challenge_type (reduction/streak/moments/screen_free)
  - target_value (int)
  - duration_days (int)
  - starts_at, ends_at (timestamptz)
  - is_active (boolean)
  - max_participants (int)

challenge_participants
  - challenge_id (FK -> challenges)
  - user_id (FK -> profiles)
  - current_progress (int)
  - completed (boolean)
  - joined_at (timestamptz)

irl_lock_sessions
  - id (uuid)
  - initiator_id (FK -> profiles)
  - partner_id (FK -> profiles)
  - duration_minutes (int, 30 or 60)
  - started_at (timestamptz)
  - ended_at (timestamptz)
  - completed (boolean)
  - moments_awarded (int)

notifications_log
  - id (uuid)
  - user_id (FK -> profiles)
  - type (streak/challenge/reward/nudge/social)
  - title (text)
  - body (text)
  - read (boolean)
  - sent_at (timestamptz)
```

---

## Project Structure

```
nomoClone/
├── apps/
│   ├── mobile/                          # React Native (Expo) app
│   │   ├── app.json                     # Expo config
│   │   ├── eas.json                     # EAS Build config
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   ├── tailwind.config.js           # NativeWind config
│   │   ├── app/                         # Expo Router (file-based routing)
│   │   │   ├── _layout.tsx              # Root layout (providers, auth gate)
│   │   │   ├── (onboarding)/
│   │   │   │   ├── _layout.tsx
│   │   │   │   ├── welcome.tsx
│   │   │   │   ├── language.tsx
│   │   │   │   ├── permissions.tsx
│   │   │   │   ├── goal.tsx
│   │   │   │   └── ready.tsx
│   │   │   ├── (auth)/
│   │   │   │   ├── _layout.tsx
│   │   │   │   ├── login.tsx
│   │   │   │   ├── signup.tsx
│   │   │   │   └── forgot-password.tsx
│   │   │   └── (tabs)/
│   │   │       ├── _layout.tsx          # Tab navigator
│   │   │       ├── index.tsx            # Home/Dashboard
│   │   │       ├── rewards/
│   │   │       │   ├── _layout.tsx
│   │   │       │   ├── index.tsx        # Rewards catalog
│   │   │       │   ├── [id].tsx         # Reward detail
│   │   │       │   └── my-rewards.tsx
│   │   │       ├── social/
│   │   │       │   ├── _layout.tsx
│   │   │       │   ├── index.tsx        # Leaderboard
│   │   │       │   ├── challenges.tsx
│   │   │       │   ├── challenge/[id].tsx
│   │   │       │   └── friends.tsx
│   │   │       └── profile/
│   │   │           ├── _layout.tsx
│   │   │           ├── index.tsx        # Profile & stats
│   │   │           ├── settings.tsx
│   │   │           └── screen-time.tsx  # Detailed breakdown
│   │   ├── components/
│   │   │   ├── ui/                      # Button, Card, Badge, Input, etc.
│   │   │   ├── home/                    # ProgressRing, MomentsCard, StreakBadge
│   │   │   ├── rewards/                 # RewardCard, CategoryPills, RedeemSheet
│   │   │   ├── social/                  # LeaderboardRow, ChallengeCard, FriendRow
│   │   │   ├── profile/                 # StatCard, BadgeGrid, ScreenTimeChart
│   │   │   ├── onboarding/             # Carousel, GoalSlider, PermissionCard
│   │   │   └── shared/                  # LanguagePicker, Avatar, EmptyState
│   │   ├── lib/
│   │   │   ├── supabase.ts             # Supabase client (AsyncStorage)
│   │   │   ├── screen-time/
│   │   │   │   ├── index.ts            # Unified API
│   │   │   │   ├── ios.ts              # iOS DeviceActivity bridge
│   │   │   │   └── android.ts          # Android UsageStats bridge
│   │   │   ├── notifications.ts        # Push notification setup
│   │   │   ├── nfc.ts                  # NFC IRL Lock logic
│   │   │   ├── i18n.ts                 # i18n-js setup
│   │   │   └── utils.ts
│   │   ├── hooks/
│   │   │   ├── useUser.ts
│   │   │   ├── useMoments.ts
│   │   │   ├── useScreenTime.ts
│   │   │   ├── useRewards.ts
│   │   │   ├── useLeaderboard.ts
│   │   │   ├── useChallenges.ts
│   │   │   └── useNotifications.ts
│   │   ├── stores/
│   │   │   ├── auth-store.ts           # Zustand auth state
│   │   │   ├── screen-time-store.ts    # Local screen time cache
│   │   │   └── settings-store.ts       # User preferences
│   │   ├── locales/
│   │   │   ├── en.json
│   │   │   ├── fr.json
│   │   │   ├── de.json
│   │   │   └── lu.json
│   │   ├── types/
│   │   │   ├── database.ts
│   │   │   ├── screen-time.ts
│   │   │   └── navigation.ts
│   │   ├── constants/
│   │   │   ├── colors.ts
│   │   │   ├── moments.ts              # Points calculation rules
│   │   │   └── config.ts
│   │   └── modules/                    # Custom Expo native modules
│   │       └── screen-time/
│   │           ├── index.ts
│   │           ├── ios/                 # Swift DeviceActivity module
│   │           └── android/             # Kotlin UsageStats module
│   │
│   └── web/                             # Next.js marketing site
│       ├── package.json
│       ├── next.config.ts
│       ├── tailwind.config.ts
│       ├── messages/{en,fr,de,lu}.json
│       ├── src/
│       │   ├── middleware.ts
│       │   ├── i18n/
│       │   ├── app/[locale]/
│       │   │   ├── page.tsx             # Landing page
│       │   │   ├── parents/page.tsx
│       │   │   ├── educators/page.tsx
│       │   │   ├── privacy/page.tsx
│       │   │   └── blog/
│       │   └── components/
│       │       ├── marketing/           # Hero, Features, HowItWorks, etc.
│       │       └── layout/              # Nav, Footer, LanguageSwitcher
│
├── packages/
│   └── shared/                          # Shared between mobile + web
│       ├── types/                       # Database types, API types
│       ├── constants/                   # Shared constants
│       └── validations/                 # Zod schemas
│
└── supabase/
    ├── config.toml
    ├── seed.sql
    ├── migrations/
    │   ├── 001_create_profiles.sql
    │   ├── 002_create_daily_logs.sql
    │   ├── 003_create_milestones.sql
    │   ├── 004_create_rewards.sql
    │   ├── 005_create_social.sql
    │   ├── 006_create_challenges.sql
    │   ├── 007_create_irl_sessions.sql
    │   ├── 008_create_notifications.sql
    │   └── 009_create_rls_policies.sql
    └── functions/
        ├── calculate-moments/index.ts   # Daily Moments calculation
        ├── refresh-leaderboard/index.ts # Leaderboard recomputation
        ├── redeem-reward/index.ts       # Atomic reward redemption
        ├── send-notifications/index.ts  # Push notification dispatch
        └── daily-streak-check/index.ts  # Streak maintenance cron
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
| `nomo-success` | `#22C55E` | Goal achieved, positive trends |
| `nomo-warning` | `#F59E0B` | Approaching limit, nudges |
| `nomo-muted` | `#94A3B8` | Secondary text, inactive states |

### Typography
- **Headings**: Bold, modern sans-serif (Inter or SF Pro on iOS, Roboto on Android)
- **Body**: Clean, readable sans-serif
- **Numbers/Stats**: Tabular lining figures for alignment

### Design Principles
- Native platform feel (iOS and Android design patterns respected)
- Bold, youthful aesthetic
- Dark/light mode support (follows system preference)
- Smooth 60fps animations (Reanimated)
- Haptic feedback on key interactions (Moments earned, IRL Lock, redemptions)
- Accessibility (minimum touch targets 44pt, VoiceOver/TalkBack support)

---

## Authentication Flow

```
App Launch
    |
    ├── No session -> Onboarding/Auth flow
    │     1. Welcome carousel (3 slides)
    │     2. Language selection
    │     3. Sign up: email/password, Google, or Apple Sign-In
    │     4. Email verification (if email signup)
    │     5. DB trigger creates profile row
    │     6. Grant screen time permission (OS dialog)
    │     7. Set daily goal
    │     8. Enable notifications
    │     9. -> Home tab
    |
    └── Has session -> Token refresh -> Home tab
          (Supabase auto-refreshes JWT via AsyncStorage)

Session management:
  - Supabase client uses @react-native-async-storage/async-storage
  - JWT auto-refresh on app foreground
  - Biometric unlock option (FaceID/TouchID) for returning users
```

---

## Implementation Phases

### Phase 1: Monorepo Scaffold & Config (Week 1)

- Initialize monorepo (turborepo or npm workspaces)
- Expo app with Expo Router, NativeWind, TypeScript
- Next.js marketing site scaffold
- Shared packages (types, constants, validations)
- Supabase project setup + client libraries
- Tailwind/NativeWind themes with Luxembourg colors
- i18n setup for both mobile (i18n-js) and web (next-intl)
- ESLint, Prettier config

### Phase 2: Database & Backend (Week 1-2)

- All 9 SQL migrations
- RLS policies
- DB trigger: auto-create profile on signup
- Seed milestones and sample Luxembourg rewards
- Supabase Edge Functions: calculate-moments, refresh-leaderboard, redeem-reward
- Generate TypeScript types

### Phase 3: Screen Time Native Module (Week 2-4) ← CRITICAL PATH

- **iOS module** (Swift):
  - FamilyControls authorization
  - DeviceActivityMonitor for real-time tracking
  - DeviceActivityReport for historical data
  - App shielding for IRL Lock
- **Android module** (Kotlin):
  - UsageStatsManager permission request
  - Usage data query and aggregation
  - App overlay for IRL Lock
- **Unified TypeScript API**:
  - `getScreenTime(date)` -> `{ total, social, apps: {...} }`
  - `requestPermission()` -> boolean
  - `lockApps(appIds, durationMinutes)` -> void
- Test on physical devices (screen time APIs don't work in simulators)

### Phase 4: Auth & Onboarding (Week 3-4)

- Auth screens (login, signup, forgot password)
- Apple Sign-In + Google Sign-In
- Onboarding carousel
- Permission grant screen (screen time + notifications)
- Goal setting screen
- Auth state management (Zustand + Supabase)
- Protected navigation (redirect to auth if no session)

### Phase 5: Home Dashboard (Week 4-5)

- Real-time screen time display (animated progress ring)
- Moments earned today/this week/total
- Streak display with flame animation
- Daily goal progress (X of Y minutes used)
- Quick action buttons (Start IRL Lock, Log Activity)
- Moments calculation integration (Edge Function)
- Pull-to-refresh
- Home screen widget (iOS + Android)

### Phase 6: Rewards System (Week 5-6)

- Rewards catalog with category filter pills
- Reward cards with images, partner logos, Moments cost
- Reward detail screen with map (location)
- Redeem flow (confirmation bottom sheet -> deduct Moments -> generate QR code)
- My Rewards screen (active vouchers with QR display)
- Reward expiration handling

### Phase 7: Social Features (Week 6-7)

- Leaderboard with tabs (daily/weekly/monthly/all-time)
- Friend system (add via username, QR code, contacts)
- Challenge creation (type, target, duration, invite friends)
- Challenge detail with participant progress bars
- IRL Lock via NFC (tap phones -> both lock social apps -> earn bonus Moments)
- Push notifications for social events

### Phase 8: Marketing Website (Week 7-8)

- Landing page (hero, features, how it works, perks, stats, app store CTAs)
- Parents page
- Educators page (B2B pilot signup form)
- Privacy policy page
- Blog scaffold
- App Store / Play Store badge links

### Phase 9: Polish, Testing, Submission (Week 8-10)

- Full i18n pass (EN complete, FR complete, DE/LU partial)
- Animations polish (Reanimated spring configs, haptics)
- Loading states, error handling, empty states
- Offline support (queue Moments sync when back online)
- Performance audit (Flipper, React DevTools)
- App Store assets (screenshots, descriptions in 4 languages)
- TestFlight beta with 5-10 Luxembourg school students
- Apple App Store submission (allow 1-2 weeks for review)
- Google Play Store submission
- Vercel deploy for marketing site

---

## Key Architecture Decisions

| Decision | Rationale |
|---|---|
| React Native (Expo) over Flutter | JavaScript ecosystem, easier to share code with Next.js web, Expo ecosystem for OTA updates |
| Expo Router (file-based) | Consistent with Next.js mental model, type-safe navigation |
| Custom native module for screen time | No reliable third-party library exists; OS APIs require direct native integration |
| Zustand over Redux | Simpler API, less boilerplate, sufficient for this app's state complexity |
| TanStack Query for server state | Caching, background refetching, optimistic updates for Moments/leaderboard |
| Monorepo (apps/mobile + apps/web + packages/shared) | Share types, constants, validations between mobile and web |
| Supabase Edge Functions for Moments calc | Server-side computation prevents cheating; atomic transactions for reward redemption |
| Per-locale DB columns for rewards | Simple, no joins needed, fast locale-aware queries |
| EAS Build for CI/CD | Expo's cloud build service handles native builds without local Xcode/Android Studio |

---

## Screen Time API Details

### iOS (DeviceActivityFramework)

```
Requirements:
  - iOS 16+ (covers 95%+ of active iPhones)
  - FamilyControls entitlement (requires Apple approval)
  - User must grant authorization via AuthorizationCenter

Data available:
  - Per-app usage duration
  - App categories (social, entertainment, productivity, etc.)
  - Number of pickups
  - Number of notifications received
  - Can shield/block specific apps (used for IRL Lock)

Limitations:
  - Apple restricts which apps can use this API
  - Must apply for FamilyControls entitlement (takes 1-4 weeks)
  - Cannot read other apps' content, only usage duration
  - Data stays in on-device container (privacy-preserving)

Alternative (simpler, less data):
  - ScreenTime API via Settings URL scheme
  - User screenshots their Screen Time and app OCRs it
  - Less accurate but no special entitlement needed
```

### Android (UsageStatsManager)

```
Requirements:
  - Android 5.0+ (API 22+)
  - PACKAGE_USAGE_STATS permission (user grants in Settings)

Data available:
  - Per-app foreground time
  - Last time app was used
  - Usage events (app opened, app closed)
  - Can aggregate by day/week/month

Limitations:
  - User must manually toggle permission in Settings (extra friction)
  - Some OEMs (Huawei, Xiaomi) restrict background access
  - Cannot block apps natively (need accessibility service or device admin)

For IRL Lock on Android:
  - Use overlay + accessibility service to show blocker
  - Or use Digital Wellbeing integration where available
```

---

## App Store Strategy

### Apple App Store

- **Category**: Health & Fitness > Mental Wellbeing
- **Entitlements needed**: FamilyControls (apply early - can take weeks)
- **In-App Purchases**: Subscription (Plus, Family tiers) via StoreKit 2
- **Age rating**: 4+ (no objectionable content)
- **App Review tips**: Emphasize parental controls angle, digital wellness focus
- **Localization**: EN, FR, DE metadata (App Store doesn't support Luxembourgish)

### Google Play Store

- **Category**: Health & Fitness
- **Permissions**: PACKAGE_USAGE_STATS, NFC, INTERNET, NOTIFICATIONS
- **Billing**: Google Play Billing Library v6 for subscriptions
- **Content rating**: Everyone
- **Data safety**: Declare minimal data collection (aggregated usage only)

---

## Environment Variables

```env
# Supabase
EXPO_PUBLIC_SUPABASE_URL=your_supabase_url
EXPO_PUBLIC_SUPABASE_ANON_KEY=your_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key

# Push Notifications
EXPO_PUBLIC_PUSH_PROJECT_ID=your_expo_project_id

# OAuth
GOOGLE_CLIENT_ID=your_google_client_id
APPLE_SERVICE_ID=your_apple_service_id

# Web (Next.js)
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_anon_key
```

---

## Business Model & Monetization

### Revenue Streams

#### 1. Freemium Subscription (Primary Revenue - via App Store / Play Store)

| Tier | Price | Features |
|---|---|---|
| **Free** | 0 | Screen time tracking, basic Moments earning, limited rewards catalog (3 per month), public leaderboard |
| **NomoLux Plus** | 4.99/month or 39.99/year | Full rewards catalog, unlimited redemptions, friend challenges, detailed per-app analytics, priority reward access, custom daily goals, export data |
| **NomoLux Family** | 9.99/month or 79.99/year | Up to 5 family members, parental dashboard (see kids' progress), family challenges, shared rewards wallet, screen time alerts for parents |

**Note**: Apple/Google take 30% commission Year 1, 15% Year 2+ (Small Business Program). Factored into projections below.

**Conversion assumption**: 4-6% free-to-paid (conservative for native app with real value prop)

#### 2. Partner Commissions (Luxembourg Local Business Network)

| Partner Type | Commission Model | Est. Revenue Per Transaction |
|---|---|---|
| Restaurants | 10-15% per voucher redeemed | 3-8 |
| Retail/Shopping | 8-12% per gift card sold | 2-10 |
| Events/Culture | 5-10% per ticket sold | 2-15 |
| Cash-back partners | 2-5% affiliate commission | 0.50-3 |

**Target**: 20-50 local Luxembourg partners in Year 1, growing to 100+ by Year 2.
Partner onboarding fee: 200 one-time setup + revenue share.

#### 3. Institutional/B2B Licensing (Schools-First Go-to-Market)

| Client | Pricing | Description |
|---|---|---|
| **Schools** | 500-2,000/year per school | Educator dashboard, class leaderboards, student wellness reports, bulk app deployment |
| **Employers** | 2-5/employee/month | Corporate wellness program, team challenges, aggregated reports |
| **Municipalities** | 5,000-15,000/year | City-wide digital wellness initiatives, public leaderboards |

Luxembourg has ~180 secondary schools and ~4,000 employers.
**Schools-first strategy**: Pilot with 5-10 lycees -> students become consumer users -> organic growth.

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

### Cost Structure (Monthly) - Native App

| Cost Item | Month 1-6 | Month 7-12 | Year 2 |
|---|---|---|---|
| Supabase (DB + Auth + Functions) | 25 | 75 | 150 |
| Vercel hosting (marketing site) | 20 | 20 | 50 |
| Apple Developer Account | 8 | 8 | 8 |
| Google Play Developer (one-time $25) | 4 | 0 | 0 |
| EAS Build (Expo) | 0 | 15 | 30 |
| Push Notification service | 0 | 10 | 30 |
| Domain + SSL | 5 | 5 | 5 |
| Email service (Resend) | 0 | 20 | 50 |
| Marketing/Ads | 500 | 2,000 | 4,000 |
| Partner acquisition | 200 | 500 | 500 |
| Customer support | 0 | 500 | 1,500 |
| Development/Maintenance | 3,000 | 3,000 | 4,000 |
| Legal/Compliance (GDPR) | 300 | 150 | 150 |
| **Total Monthly Costs** | **4,062** | **6,303** | **10,473** |

### Revenue Model by User Count

**Assumptions:**
- Luxembourg target demographic (12-35): ~180,000
- Free-to-paid conversion: 5% (native app with real tracking = higher perceived value)
- App Store commission: 25% blended (30% Y1 / 15% Y2+)
- Net subscription revenue per user: 4.99 * 0.75 = **3.74/month**
- Average partner commission per paying user: **5/month** (1.5 redemptions)
- B2B revenue starts Month 7 (schools-first strategy)

#### Scenario: User Growth & Revenue

| Users (Total) | Paying (5%) | Subscriptions/mo (net) | Partner Rev/mo | B2B Rev/mo | **Total Rev/mo** | Monthly Costs | **Net/mo** |
|---|---|---|---|---|---|---|---|
| **500** | 25 | 94 | 63 | 0 | **156** | 4,062 | **-3,906** |
| **1,000** | 50 | 187 | 125 | 500 | **812** | 4,300 | **-3,488** |
| **2,500** | 125 | 468 | 313 | 1,500 | **2,280** | 5,000 | **-2,720** |
| **5,000** | 250 | 935 | 625 | 3,000 | **4,560** | 5,800 | **-1,240** |
| **7,500** | 375 | 1,403 | 938 | 4,500 | **6,840** | 6,303 | **+537** |
| **10,000** | 500 | 1,870 | 1,250 | 5,500 | **8,620** | 7,000 | **+1,620** |
| **15,000** | 750 | 2,805 | 1,875 | 7,000 | **11,680** | 8,000 | **+3,680** |
| **25,000** | 1,250 | 4,675 | 3,125 | 10,000 | **17,800** | 10,473 | **+7,327** |
| **50,000** | 2,500 | 9,350 | 6,250 | 15,000 | **30,600** | 14,000 | **+16,600** |

### Break-Even Analysis

```
BREAK-EVEN POINT: ~7,000-7,500 total users
                   (~350-375 paying subscribers)
                   + 3-5 school B2B contracts

Timeline (schools-first go-to-market):
  Month 1-2:   Development sprint, no users
  Month 3-4:   Beta with 3-5 Luxembourg lycees, 200-500 users
  Month 5-6:   Public launch on App Store/Play Store, 500-2,000 users
  Month 7-9:   School program expands (10-15 schools), 2,000-4,000 users
  Month 10-12: Partner network live, organic growth, 4,000-7,000 users
  Month 12-14: BREAK-EVEN at ~7,500 users
  Month 18:    12,000 users, profitable at ~2,500/month
  Month 24:    25,000 users, ~7,300/month profit

TOTAL INVESTMENT TO BREAK-EVEN: ~65,000-75,000
```

### Revenue Composition at Scale (25,000 users)

```
Subscriptions (net):  4,675/mo   (26%)  █████████
Partner Revenue:      3,125/mo   (18%)  ██████
B2B Licensing:       10,000/mo   (56%)  ████████████████████
Sponsored:           variable          (bonus)
                     ─────────
Total:              17,800/mo   (100%)

Annual Revenue:     ~213,600
Annual Costs:       ~125,700
Annual Profit:      ~87,900

Note: B2B is the dominant revenue stream. This validates
the schools-first strategy. Consumer subscriptions are
gravy, not the main course.
```

### Luxembourg Market Opportunity

| Metric | Value |
|---|---|
| Total population | ~660,000 |
| Target demographic (12-35) | ~180,000 |
| Realistic addressable market (smartphone owners in target) | ~150,000 |
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
- **Government alignment**: Digital wellness is a policy priority

### Key Metrics to Track (KPIs)

| Metric | Target (Month 6) | Target (Month 12) | Target (Month 24) |
|---|---|---|---|
| App downloads | 2,500 | 8,000 | 30,000 |
| Monthly Active Users (MAU) | 1,500 | 5,000 | 20,000 |
| DAU/MAU ratio | 40% | 50% | 55% |
| Paid subscribers | 75 | 400 | 1,250 |
| Free-to-paid conversion | 5% | 5% | 5% |
| Monthly churn rate | <10% | <6% | <4% |
| Partner businesses | 10 | 35 | 80 |
| School contracts | 5 | 15 | 40 |
| Avg. Moments earned/user/day | 30 | 60 | 80 |
| Avg. daily screen time reduction | 12 min | 22 min | 30 min |
| App Store rating | 4.2+ | 4.5+ | 4.6+ |
| D7 retention | 40% | 50% | 55% |
| D30 retention | 20% | 30% | 35% |

### Funding Requirements

| Phase | Amount | Purpose |
|---|---|---|
| **Pre-seed (Month 0-2)** | 20,000 | MVP development, Apple entitlements, legal setup |
| **Seed (Month 3-6)** | 30,000 | School pilot program, marketing launch, beta testing |
| **Growth (Month 7-14)** | 25,000 | Scale marketing, partner acquisition, B2B sales |
| **Total to profitability** | **75,000** | Covers runway until ~Month 13 break-even |

Luxembourg-specific funding sources:
- **Fit4Start** accelerator (up to 150,000 + coaching)
- **Luxembourg National Research Fund (FNR)** grants
- **Luxinnovation** support programs
- **Digital Luxembourg** initiative alignment
- **Banque de Luxembourg** startup programs
- **SNCI** (Societe Nationale de Credit et d'Investissement) startup loans

---

## Risk Assessment

| Risk | Impact | Likelihood | Mitigation |
|---|---|---|---|
| Apple rejects FamilyControls entitlement | Critical | Medium | Apply early (Month 1); fallback: manual input + Screen Time screenshot OCR |
| Low retention after onboarding | High | Medium | Rich onboarding, daily notifications, streak gamification, school peer pressure |
| Partner businesses slow to sign up | Medium | Medium | Seed rewards with own budget; start with 5 partners, prove ROI, then scale |
| Screen time API changes (iOS/Android) | High | Low | Abstract native module; monitor Apple/Google developer betas annually |
| Competition (Apple Screen Time, Google Wellbeing built-in) | Medium | High | Differentiate via rewards + social + Luxembourg focus; built-in tools have no gamification |
| GDPR compliance complexity | Medium | Low | Privacy-by-design: no individual data leaves device; only aggregates sent to server |

---

## Future Enhancements (Post-MVP)
- Apple Watch / Wear OS companion app
- Push notification AI (optimal timing for nudges using ML)
- NFC-based IRL Lock with custom animations
- Partner portal web app for businesses to manage rewards
- White-label version for other EU small countries (Malta, Estonia, Cyprus)
- Integration with Apple Health / Google Fit
- AI-powered screen time insights ("You use Instagram most after 9pm on weekdays")
- Parental controls dashboard (web + mobile)
- Supabase Realtime for live leaderboard animations
- Offline-first architecture with conflict resolution
