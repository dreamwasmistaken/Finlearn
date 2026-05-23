# WealthLift

A production-ready financial education platform for ages **13–25**. Dark space-themed UI, gamified lessons, live stock markets, TradingView-style charts, AI tutor, Stripe subscriptions, and Supabase auth/database.

## Features

| Feature | Description |
|---------|-------------|
| **Courses & Lessons** | Money, Business, Investing, Mindset, Financial Intelligence |
| **Rich lesson pages** | Explanation, key insight, examples, billionaire thinking, mistakes, actions, quiz |
| **Dynamic quizzes** | New questions generated on every attempt after completion |
| **Gamification** | XP, levels, streaks, progress tracking |
| **Live Markets** | Real-time quotes (Finnhub or Yahoo Finance fallback), 30s refresh |
| **AI Terminal** | Candlestick charts + AI learning tutor (Premium) |
| **Stripe** | Free vs Premium subscription |
| **Auth** | Supabase email/password |

## Tech Stack

- **Frontend:** Next.js 14, React, Tailwind CSS
- **Backend:** Supabase (Auth + PostgreSQL)
- **Payments:** Stripe Checkout + Webhooks
- **Charts:** lightweight-charts (TradingView-style)
- **Deploy:** Vercel

## Quick Start

### 1. Install Node.js

Install [Node.js 20+](https://nodejs.org/) (includes npm).

### 2. Install dependencies

```bash
cd wealthlift
npm install
```

### 3. Environment variables

Copy `.env.example` to `.env.local` and fill in:

```bash
cp .env.example .env.local
```

| Variable | Required | Purpose |
|----------|----------|---------|
| `NEXT_PUBLIC_SUPABASE_URL` | Yes | Supabase project URL |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Yes | Supabase anon key |
| `SUPABASE_SERVICE_ROLE_KEY` | For webhooks | Stripe webhook profile updates |
| `STRIPE_SECRET_KEY` | For billing | Checkout |
| `STRIPE_PREMIUM_PRICE_ID` | For billing | Monthly price ID |
| `STRIPE_WEBHOOK_SECRET` | For billing | Webhook signature |
| `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY` | Optional | Client Stripe |
| `FINNHUB_API_KEY` | Recommended | Live market data ([finnhub.io](https://finnhub.io)) |
| `OPENAI_API_KEY` | Optional | Smarter AI tutor (template fallback included) |
| `NEXT_PUBLIC_APP_URL` | Yes (prod) | e.g. `https://your-app.vercel.app` |

### 4. Supabase setup

1. Create a project at [supabase.com](https://supabase.com)
2. Open **SQL Editor** → run `supabase/schema.sql`
3. Enable **Email** auth under Authentication → Providers
4. Add your site URL under Authentication → URL Configuration

> **Note:** Lessons work immediately via built-in seed data (`src/lib/seed-data.ts`). Database seed is optional for admin/editing in Supabase.

### 5. Stripe setup

1. Create products at [dashboard.stripe.com](https://dashboard.stripe.com)
2. Create a **$9/month** recurring price → copy `price_...` to `STRIPE_PREMIUM_PRICE_ID`
3. For local webhooks: `stripe listen --forward-to localhost:3000/api/stripe/webhook`
4. Copy webhook signing secret to `STRIPE_WEBHOOK_SECRET`

### 6. Run locally

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

## Project Structure

```
wealthlift/
├── src/
│   ├── app/              # Pages & API routes
│   │   ├── api/          # stocks, quiz, progress, stripe, ai
│   │   ├── dashboard/
│   │   ├── courses/
│   │   ├── lessons/
│   │   ├── markets/      # Live 24/7 quotes
│   │   └── terminal/     # Charts + AI tutor
│   ├── components/
│   └── lib/              # seed data, markets, quiz generator
├── supabase/
│   ├── schema.sql
│   └── seed.sql
└── .env.example
```

## Deploy to Vercel

1. Push to GitHub
2. Import repo in [vercel.com](https://vercel.com)
3. Set root directory to `wealthlift`
4. Add all environment variables
5. Deploy

Add production URL to Supabase auth redirect URLs and Stripe webhook endpoint:
`https://your-domain.com/api/stripe/webhook`

## Plans

| Free | Premium ($9/mo) |
|------|-----------------|
| Core courses | All courses including Investing |
| Basic quizzes | Fresh quiz questions every attempt |
| Live markets (logged in) | AI Trading Terminal tutor |
| Dashboard & XP | Full premium content |

## Market Data

- **Primary:** Finnhub API (set `FINNHUB_API_KEY`)
- **Fallback:** Yahoo Finance chart API (no key, may have rate limits)

Quotes refresh every **30 seconds** on Markets and Terminal pages.

## Quiz Regeneration

Each time a user opens a lesson quiz or clicks **New questions**, the server generates a unique set from lesson content (`src/lib/quiz-generator.ts`). Results are stored in `quiz_attempts`.

## Disclaimer

WealthLift is for **financial education only**, not investment advice. Market data may be delayed. Users should consult qualified professionals before making financial decisions.

## License

Proprietary — WealthLift MVP for startup use.
