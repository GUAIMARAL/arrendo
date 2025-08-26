[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github)](https://github.com/GUAIMARAL/arrendo/releases)

# Arrendo — Rental Management for Small Landlords and Managers 🏘️

Open-source rental management system for small landlords and property managers. Manage properties, tenants, leases, and payments with automation and an intuitive dashboard. Built with NestJS (backend) and React (frontend). Self-host or run in a small cloud VM.

---

## Table of contents

- Features
- Screenshots
- Tech stack
- Architecture
- Quick start (local)
- Production (Docker)
- Releases
- Configuration
- CLI & useful commands
- Contributing
- Roadmap
- FAQ
- License

---

## Features

- Property inventory: units, buildings, addresses, custom fields.
- Tenant records: contacts, documents, communication log.
- Lease management: terms, rent schedule, renewals, automatic notifications.
- Rent and payment tracking: multiple payment methods, manual posting, ledger.
- Automated reminders: email and optional SMS for rent and maintenance.
- Reports: occupancy, arrears, cashflow, tax-ready export (CSV).
- Roles & permissions: owner, manager, accountant, maintenance.
- Web dashboard: responsive React UI for desktop and mobile.
- API-first: REST endpoints and optional GraphQL layer for integrations.
- Self-host friendly: Docker and Compose for simple deployment.

---

## Screenshots

![Dashboard - rent overview](https://images.unsplash.com/photo-1560185127-6b4f3a5b2a7a?q=80&w=1200&auto=format&fit=crop)
_Clean dashboard with portfolio summary._

![Tenant profile](https://images.unsplash.com/photo-1524758631624-e2822e304c36?q=80&w=1200&auto=format&fit=crop)
_Tenant profile and lease timeline._

---

## Tech stack

- Backend: NestJS, TypeScript, Node.js
- ORM: Prisma or TypeORM (configurable)
- Database: PostgreSQL
- Cache / Jobs: Redis (optional)
- Frontend: React, TypeScript, Vite / Create React App
- Auth: JWT, optional OAuth providers
- Payments: Stripe integration (optional)
- Container: Docker, Docker Compose

---

## Architecture

- Backend (NestJS)
  - Modules: auth, properties, tenants, leases, payments, notifications
  - Services handle business logic; controllers expose REST endpoints
  - Background jobs for invoicing and reminders
- Frontend (React)
  - Pages: Dashboard, Properties, Tenants, Leases, Payments, Reports
  - Shared component library and hooks for API calls
- Integration
  - Webhooks for payments and external services
  - Export endpoints for accounting systems

Diagram (simple):
- Client (React) -> API Gateway (NestJS) -> Database (Postgres)
- Jobs queue -> Redis -> Workers (NestJS processes)

---

## Quick start (local)

Prerequisites:
- Node.js 18+ and npm or yarn
- PostgreSQL 12+
- Redis (optional for background jobs)
- Git

Clone and run:

1. Clone repository
```bash
git clone https://github.com/GUAIMARAL/arrendo.git
cd arrendo
```

2. Copy env files and edit
```bash
cp .env.example .env
# open .env and set DATABASE_URL, JWT_SECRET, and other keys
```

3. Install dependencies (backend and frontend)
```bash
# from repo root
yarn install
# Or: npm install
```

4. Run database migrations and seed (example using Prisma)
```bash
# backend
cd backend
yarn prisma:migrate
yarn prisma:seed
```

5. Start backend and frontend in development
```bash
# from repo root
# start backend
yarn workspace @arrendo/backend dev
# start frontend
yarn workspace @arrendo/frontend dev
```

6. Open http://localhost:3000 (or port shown in your .env)

Testing:
```bash
# run unit and integration tests
yarn test
```

---

## Production (Docker)

Use Docker Compose for a single-server production deploy.

Example docker-compose.yml (simplified)
```yaml
version: "3.8"
services:
  db:
    image: postgres:15
    environment:
      POSTGRES_USER: arrendo
      POSTGRES_PASSWORD: arrendo
      POSTGRES_DB: arrendo
    volumes:
      - db-data:/var/lib/postgresql/data

  redis:
    image: redis:7
    volumes:
      - redis-data:/data

  backend:
    image: ghcr.io/youruser/arrendo-backend:latest
    depends_on:
      - db
      - redis
    environment:
      DATABASE_URL: postgres://arrendo:arrendo@db:5432/arrendo
      REDIS_URL: redis://redis:6379
      NODE_ENV: production
    ports:
      - "4000:4000"

  frontend:
    image: ghcr.io/youruser/arrendo-frontend:latest
    depends_on:
      - backend
    ports:
      - "80:80"

volumes:
  db-data:
  redis-data:
```

Build and run
```bash
docker compose up -d --build
```

---

## Releases

Download the packaged release and run the included installer or container scripts. Visit the releases page and pick the asset for your platform.

Download and execute the release from:
https://github.com/GUAIMARAL/arrendo/releases

Example (Linux tarball):
```bash
# download latest release tarball (replace URL with chosen asset)
curl -Lo arrendo.tar.gz "https://github.com/GUAIMARAL/arrendo/releases/download/vX.Y.Z/arrendo-linux-amd64.tar.gz"
tar -xzf arrendo.tar.gz
cd arrendo
chmod +x install.sh
./install.sh
```

If the link is not reachable, check the "Releases" section on the repository page.

---

## Configuration

Key environment variables
- DATABASE_URL — PostgreSQL connection string
- REDIS_URL — Redis connection string (optional)
- JWT_SECRET — secret for token signing
- NODE_ENV — development or production
- STRIPE_SECRET — Stripe API key (optional)
- EMAIL_SMTP_* — SMTP details for email notifications

Secrets live in environment variables. Use a secrets manager in production.

---

## CLI & useful commands

Backend
- yarn workspace @arrendo/backend start
- yarn workspace @arrendo/backend build
- yarn workspace @arrendo/backend prisma:migrate

Frontend
- yarn workspace @arrendo/frontend start
- yarn workspace @arrendo/frontend build
- yarn workspace @arrendo/frontend lint

Database
- psql $DATABASE_URL -f ./sql/schema.sql
- docker exec -it arrendo_db psql -U arrendo -d arrendo

Jobs
- yarn workspace @arrendo/worker start
- pm2 start ecosystem.config.js (example)

---

## Contributing

- Fork the repo.
- Create feature branch: git checkout -b feat/your-feature
- Add tests for new features.
- Open a pull request with a clear description and changelog entry.
- Follow code style (prettier, eslint). Run linters before PR.

Code of conduct: Be respectful. Use clear commit messages.

---

## Roadmap

Planned items:
- Mobile app (React Native)
- Multi-tenant hosting mode
- Built-in accounting exports (QuickBooks, Xero)
- Improved reconciliation and bulk payments
- Third-party integrations marketplace

Watch the Releases page and issues for progress.

---

## FAQ

Q: Can I self-host on a small VPS?
A: Yes. Use Docker Compose or run Node processes behind a reverse proxy.

Q: Do you support multiple currencies?
A: The model supports currency fields. Payment provider support depends on your Stripe account.

Q: How do I import existing tenant data?
A: Use the CSV import tool in the admin panel or the import API endpoint.

Q: Is there a SaaS offering?
A: This repo focuses on the self-hosted stack. You can adapt containers to run in your cloud account.

---

## License

Arrendo uses the MIT license. See LICENSE file for details.

---

Support links
- Issues: https://github.com/GUAIMARAL/arrendo/issues
- Releases: https://github.com/GUAIMARAL/arrendo/releases

Topics: lease-management, nestjs, open-source, property-management, property-rental, react, rent-payment-tracking, rental-management, saas, self-hosted, tenant-tracking