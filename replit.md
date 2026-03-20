# Workspace

## Overview

pnpm workspace monorepo using TypeScript. Each package manages its own dependencies.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)
- **Telegram Bot**: grammY

## Structure

```text
artifacts-monorepo/
├── artifacts/              # Deployable applications
│   ├── api-server/         # Express API server
│   └── telegram-bot/       # LUM Bank Telegram Bot (grammY)
├── lib/                    # Shared libraries
│   ├── api-spec/           # OpenAPI spec + Orval codegen config
│   ├── api-client-react/   # Generated React Query hooks
│   ├── api-zod/            # Generated Zod schemas from OpenAPI
│   └── db/                 # Drizzle ORM schema + DB connection
├── scripts/                # Utility scripts
├── pnpm-workspace.yaml
├── tsconfig.base.json
├── tsconfig.json
└── package.json
```

## Telegram Bot — LUM Bank

Located in `artifacts/telegram-bot/`. A game bank bot with its own LUM currency and economic system.

### Environment Variables Required

- `TELEGRAM_BOT_TOKEN` — secret from @BotFather
- `ADMIN_TELEGRAM_ID` — env var, the admin's Telegram user ID
- `DATABASE_URL` — auto-provisioned by Replit PostgreSQL

### Database Tables

- `lum_users` — user profiles, balances, trust levels
- `lum_transactions` — full transaction history
- `lum_economy` — economy state (inflation, multiplier, crisis)

### User Commands

- `/start` — register and get 1000 LUM starting balance
- `/balance` — show balance and profile
- `/work` — earn LUM (1 hour cooldown)
- `/send [id] [amount]` — transfer LUM to another user
- `/top` — top-10 leaderboard
- `/history` — last 10 transactions
- `/economy` — current economy state
- `/profile [id]` — view another user's profile

### Admin Commands (ADMIN_TELEGRAM_ID only)

- `/give [id] [amount]` — give LUM to user
- `/take [id] [amount]` — take LUM from user
- `/settrust [id] [1-10]` — set trust level
- `/setmult [number]` — set work multiplier
- `/setinflation [number]` — set inflation rate
- `/crisis [name]` — trigger economic crisis
- `/endcrisis` — end current crisis
- `/broadcast [text]` — send message to all users
- `/adminstat` — system statistics

### Economy Features

- Inflation auto-adjusts based on total money supply
- Random economic events (crises, booms) trigger hourly
- Daily interest (0.5%) for balances ≥ 5000 LUM
- 2% transfer tax on all transactions
- Trust levels 1–10 (affect work income)
- Work streak bonuses (up to +50%)

### Run

```
pnpm --filter @workspace/telegram-bot run start
```
