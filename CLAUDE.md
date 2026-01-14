# CLAUDE.md - Selva Delivery Orchestrator

## Project Overview

**Selva Delivery** is an offline-first delivery platform for the Peruvian jungle region (Yurimaguas, Loreto). This is the root orchestrator for the polyrepo architecture.

## Architecture

```
GitHub Organization: selva-delivery/
│
├── selva-api              # Backend FastAPI + GraphQL
├── selva-mobile           # React Native App (iOS + Android)
├── selva-web              # React Admin Panel
├── selva-ui               # Design System (Turborepo monorepo)
├── selva-docs             # Documentation + Schemas
└── selva-infra            # Docker infrastructure
```

## Repositories

| Repository | Description | Status | CLAUDE.md |
| ---------- | ----------- | ------ | --------- |
| [selva-api](https://github.com/selva-delivery/selva-api) | Backend GraphQL API (Python/FastAPI) | Module 6 Complete (565 tests, 80% coverage) | [View](./selva-api/CLAUDE.md) |
| [selva-mobile](https://github.com/selva-delivery/selva-mobile) | Mobile app (React Native/Expo) | Auth + Consumer Flow (214 tests) | [View](./selva-mobile/CLAUDE.md) |
| [selva-web](https://github.com/selva-delivery/selva-web) | Admin panel (React) | Pending | [View](./selva-web/CLAUDE.md) |
| [selva-ui](https://github.com/selva-delivery/selva-ui) | Design system (Turborepo) | v2.0.0 Published | [View](./selva-ui/CLAUDE.md) |
| selva-docs | Documentation + GraphQL schema | Active | [View](./selva-docs/CLAUDE.md) |
| selva-infra | Docker infrastructure | Active | [View](./selva-infra/CLAUDE.md) |

## Tech Stack

| Layer | Technology |
| ----- | ---------- |
| **Backend** | Python 3.14, FastAPI, Strawberry GraphQL, SQLAlchemy 2.0, PostgreSQL 18, Redis 8 |
| **Mobile** | React Native 0.83+, Expo SDK 50+, Zustand, WatermelonDB, PowerSync |
| **Web** | React 19, TanStack Query, TailwindCSS |
| **Design System** | @selva-delivery/tokens, @selva-delivery/ui-native, @selva-delivery/ui-web |
| **Infrastructure** | Docker, GitHub Actions, EAS Build |

## Design System (@selva-delivery/\*)

Published to GitHub Packages. See [selva-ui/CLAUDE.md](./selva-ui/CLAUDE.md) for details.

### Packages

| Package | Version | Description |
| ------- | ------- | ----------- |
| `@selva-delivery/tokens` | 2.0.0 | Design tokens (colors, typography, spacing) |
| `@selva-delivery/ui-native` | 2.0.0 | React Native components |
| `@selva-delivery/ui-web` | 2.0.0 | React web components |

### Installation

```bash
# Configure npm for GitHub Packages (one-time)
echo "@selva-delivery:registry=https://npm.pkg.github.com" >> .npmrc
echo "//npm.pkg.github.com/:_authToken=\${NODE_AUTH_TOKEN}" >> .npmrc

# For mobile apps
npm install @selva-delivery/tokens @selva-delivery/ui-native

# For web apps
npm install @selva-delivery/tokens @selva-delivery/ui-web
```

### Brand Colors

| Token | Value | Usage |
| ----- | ----- | ----- |
| `colors.primary.500` | #4CAF50 | Main brand (Selva green) |
| `colors.secondary.500` | #FF9800 | Action/delivery (orange) |
| `colors.success.main` | #4CAF50 | Success states |
| `colors.error.main` | #F44336 | Error states |

## User Profiles

| Profile | Mobile | Web | Description |
| ------- | ------ | --- | ----------- |
| Consumidor | Yes | No | Orders food, tracks deliveries |
| Repartidor | Yes | No | Accepts and delivers orders |
| Admin Negocio | Yes | Yes | Manages store, products, orders |
| Super Admin | Yes | Yes | Platform administration |

## Implementation Phases

### Phase 1: Foundation (COMPLETED)

- [x] selva-api: Database + GraphQL + Auth
- [x] selva-infra: Docker setup
- [x] selva-ui: Design system published

### Phase 2: Consumer (IN PROGRESS)

- [x] selva-api: Catalog, Cart, Orders APIs
- [x] selva-mobile: Multi-role auth (roles[], activeRole, RoleSwitcher)
- [x] selva-mobile: WelcomeScreen + LoginScreen (Rappi-style)
- [x] selva-mobile: Home, search, cart, checkout
- [ ] selva-mobile: Real-time order tracking

### Phase 3: Deliverer

- [x] selva-api: Deliverer APIs
- [ ] selva-mobile: Deliverer registration, orders, navigation

### Phase 4: Business Admin

- [x] selva-api: Business Admin APIs
- [ ] selva-mobile: Business dashboard
- [ ] selva-web: Full business panel

### Phase 5: Super Admin

- [x] selva-api: Super Admin APIs (complete)
- [ ] selva-mobile: Admin dashboard
- [ ] selva-web: Full admin panel

### Phase 6: Integrations

- [ ] Push notifications (FCM + SMS)
- [ ] Offline maps (Yurimaguas)
- [ ] PowerSync integration
- [ ] Production deployment

## Quick Start

### Start Infrastructure

```bash
cd selva-infra
make up-db  # PostgreSQL + Redis
```

### Start API

```bash
cd selva-api
poetry install
poetry run alembic upgrade head
poetry run uvicorn app.main:app --reload --port 8000
```

### Start Mobile

```bash
cd selva-mobile
npm install
npm start
```

### Start Web

```bash
cd selva-web
npm install
npm run dev
```

## GraphQL Schema Sync

The GraphQL schema is the source of truth. When API changes:

```bash
# Export from API
cd selva-api
poetry run strawberry export-schema app.graphql.schema:schema > ../selva-docs/schemas/schema.graphql

# Generate types in clients
cd ../selva-mobile
npm run codegen

cd ../selva-web
npm run codegen
```

## API Endpoints

| Environment | API URL | GraphQL |
| ----------- | ------- | ------- |
| Development | http://localhost:8000 | http://localhost:8000/graphql |
| Staging | https://api-staging.selvadelivery.com | /graphql |
| Production | https://api.selvadelivery.com | /graphql |

## Key Documents

| Document | Location | Description |
| -------- | -------- | ----------- |
| Plan General | [selva-docs/00-selva-delivery-plan-general.md](./selva-docs/00-selva-delivery-plan-general.md) | Project roadmap |
| API Spec | [selva-api/CLAUDE.md](./selva-api/CLAUDE.md) | Backend documentation |
| Mobile Spec | [selva-mobile/02-selva-mobile-spec.md](./selva-mobile/02-selva-mobile-spec.md) | Mobile app spec |
| Design System | [selva-ui/CLAUDE.md](./selva-ui/CLAUDE.md) | UI components |

## Development Workflow

### Adding a Feature

1. **API First**: Implement in selva-api with tests
2. **Export Schema**: Update selva-docs/schemas/schema.graphql
3. **Generate Types**: Run codegen in clients
4. **Implement UI**: Use selva-ui components
5. **Test**: Unit + integration tests

### Releasing

1. **API**: Push to main triggers CI, manual deploy to Railway
2. **Mobile**: Tag `v*` triggers EAS build
3. **Web**: Push to main auto-deploys to Vercel
4. **UI**: Use changesets + CLI publish

## Environment Variables

Each repository has its own `.env.example`. Key variables:

### selva-api

```env
DATABASE_URL=postgresql+asyncpg://postgres:postgres@localhost:5432/selva_delivery
REDIS_URL=redis://localhost:6379/0
JWT_SECRET_KEY=your-secret-key
```

### selva-mobile

```env
API_URL=http://localhost:8000
WS_URL=ws://localhost:8000
POWERSYNC_URL=https://xxxxx.powersync.co
```

### selva-ui (publishing)

```env
NODE_AUTH_TOKEN=$(gh auth token)
```

## Troubleshooting

### Cross-Platform Issues (Windows/Linux CI)

- Remove `package-lock.json` from git for repos with native dependencies
- Use `npm install` instead of `npm ci` in CI

### Prettier/Lint Failures

```bash
npm run format
```

### Docker Services Not Starting

```bash
cd selva-infra
make down && make up-db
```

### GraphQL Types Out of Sync

```bash
# Re-export and regenerate
cd selva-api && poetry run strawberry export-schema app.graphql.schema:schema > ../selva-docs/schemas/schema.graphql
cd ../selva-mobile && npm run codegen
```

## Contact & Resources

- **GitHub Org**: https://github.com/selva-delivery
- **API Docs**: http://localhost:8000/graphql (Playground)
- **Metrics**: http://localhost:8000/metrics (Prometheus)
