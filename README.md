# PokéVault v25

Pokémon card & sealed product portfolio tracker with live TCG prices, price history chart, trade analyser, and **public portfolio sharing**.

## What's new in v25

| Area | Change |
|---|---|
| **Schema** | `supabase_schema_v25.sql` — drops + recreates all tables cleanly. Adds `share_enabled` and `share_slug` columns to `profiles`. Adds public RLS policies for shared portfolios. Removes any `password_hash` concept (Supabase Auth handles all password hashing — never store passwords yourself). |
| **API fix** | All direct Supabase REST `fetch()` calls now pass **both** `apikey` and `Authorization` headers. Missing `apikey` was the cause of `{"message":"No API key found in request"}`. A `sbHeaders()` helper in `app.js` standardises this. |
| **New route** | `api/portfolio.js` — public GET `/api/portfolio?slug=<slug>` returns a user's profile + non-sold items when `share_enabled = true`. Purchase prices are stripped from the public response. |
| **New page** | `public/portfolio.html` — rendered at `/p/<slug>`. Shows card grid with current values (no purchase prices). |
| **Sharing UI** | "🔗 Share" button in the header opens a modal to toggle sharing on/off and set a custom URL slug. |
| **vercel.json** | Added `/api/portfolio` route and `/p/:slug → /public/portfolio.html` rewrite. |

## Environment variables (Vercel dashboard)

| Variable | Description |
|---|---|
| `SUPABASE_URL` | Your Supabase project URL |
| `SUPABASE_ANON_KEY` | Supabase anon/public key |
| `SUPABASE_SERVICE_KEY` | Supabase service_role key (server-only) |
| `POKEPRICE_API_KEY` | PokémonPriceTracker API bearer token |

## Deploy

```bash
git add -A
git commit -m "v25: schema v25, API key fix, portfolio sharing"
git push
# Vercel auto-deploys on push
```

## Database migration

Run `supabase_schema_v25.sql` in the Supabase SQL Editor.

> ⚠️ The script **drops all existing tables** before recreating them. Export your data first if needed.  
> To upgrade from v17 without a full reset, see the `MIGRATION NOTE` at the bottom of the SQL file.
