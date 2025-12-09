# HQ Operations - Enterprise Management System

A unified, scalable headquarters application that consolidates multiple business systems into a single command center for enterprise operations management.

## 🎯 Overview

This monorepo serves as the central hub for managing:

- **Event Management** (Battle Boards)
- **Sales Operations** (Sales Handoff)
- **Kitchen Operations** (Kitchen Manager)
- **Warehouse Management**
- **Customer Relationship Management (CRM)**
- **Task Management** (KannBahn)
- **Staff Scheduling** (GoodShuffle, Nowsta)
- **Payment Processing** (Stripe)
- **Event Planning** (Total Party Planner)
- **Agentic AI** (npcpy)

## 🏗️ Technology Stack

- **Frontend**: Next.js 16, React 19, TypeScript
- **Mobile**: React Native
- **Backend**: Inngest (serverless event-driven)
- **Database**: Supabase (PostgreSQL) + Prisma ORM
- **Validation**: Zod
- **Auth**: Oslo.js
- **Styling**: Tailwind CSS + shadcn/UI
- **Storage**: AWS S3
- **Payments**: Stripe
- **Deployment**: Vercel
- **CDN**: Cloudflare

## 📁 Repository Structure

```
hq-operations/
├── apps/
│   ├── web/                    # Next.js web dashboard
│   ├── mobile/                 # React Native mobile app
│   └── api/                    # Inngest serverless functions
├── packages/
│   ├── database/              # Prisma schema & migrations
│   ├── shared/                # Types, utilities, schemas
│   ├── ui/                    # Reusable UI components
│   └── config/                # ESLint, TypeScript, Tailwind
├── docker/                    # Docker configuration
├── .github/                   # CI/CD workflows
├── docs/
│   ├── ENTERPRISE_ARCHITECTURE.md
│   └── ARCHITECTURE_DIAGRAMS.md
└── turbo.json                 # Monorepo build config
```

## 🚀 Quick Start

### Prerequisites
- Node.js 18+
- pnpm (package manager)
- Git

### Installation

```bash
# Clone the repository
git clone https://github.com/your-username/hq-operations.git
cd hq-operations

# Install dependencies
pnpm install

# Setup environment variables
cp .env.example .env.local

# Run database migrations
pnpm db:migrate

# Start development
pnpm dev
```

### Development Commands

```bash
# Start all apps in development mode
pnpm dev

# Build all apps
pnpm build

# Run tests
pnpm test

# Run linting
pnpm lint

# Format code
pnpm format

# Database commands
pnpm db:migrate
pnpm db:seed
pnpm db:push
```

## 📋 Core Modules

### 1. Battle Board (Event Management)
- Real-time event monitoring
- Shift management and assignments
- Integrated notifications
- Integration with GoodShuffle & Nowsta

### 2. Sales Handoff (Pipeline Management)
- Deal tracking and management
- Sales pipeline visualization
- Contact and activity logging
- Automated notifications

### 3. Kitchen Manager (Operations)
- Order management and tracking
- Inventory control
- Kitchen alerts and notifications
- Real-time status updates

### 4. Warehouse Management
- Stock level management
- Pick and pack operations
- Shipment tracking
- Inventory forecasting

### 5. CRM System
- Contact management
- Deal pipeline
- Activity tracking
- Lead scoring with AI

### 6. Scheduling System
- Staff shift management
- Availability tracking
- Integration with GoodShuffle & Nowsta
- Automated notifications

### 7. Task Management (KannBahn)
- Kanban board
- Task assignment and tracking
- Team collaboration
- Real-time updates

### 8. AI Assistant (npcpy)
- Anomaly detection
- Predictive recommendations
- Cross-module insights
- Automated reporting

## 🔐 Authentication & Authorization

Uses **Oslo.js** for secure, vendor-independent authentication:
- JWT token management
- OAuth2 support
- WebAuthn for passwordless auth
- Role-based access control (RBAC)

## 📊 Database

Powered by **Supabase** (PostgreSQL) with **Prisma** ORM:
- Type-safe queries
- Automated migrations
- Real-time subscriptions
- Built-in security

## 🔄 Real-Time Events

**Inngest** handles event-driven orchestration:
- Async task processing
- Cross-module communication
- Message queue with retry logic
- Webhooks and integrations

## 📈 Monitoring & Observability

- Error tracking (Sentry)
- Performance monitoring (Web Vitals)
- Structured logging
- User analytics
- Health checks

## 🔒 Security

- Data encryption (in transit & at rest)
- Secure secret management
- API rate limiting
- CORS configuration
- Regular security audits
- Dependency scanning

## 📚 Documentation

- [Enterprise Architecture](./docs/ENTERPRISE_ARCHITECTURE.md) - System design, database schema, implementation phases
- [Architecture Diagrams](./docs/ARCHITECTURE_DIAGRAMS.md) - 9 detailed Mermaid diagrams

## 🚢 Deployment

### Staging
```bash
vercel deploy --env staging
```

### Production
```bash
vercel deploy --prod
```

### Database Migrations
```bash
pnpm db:deploy
```

## 🤝 Contributing

Please read [CONTRIBUTING.md](.github/CONTRIBUTING.md) for details on our code of conduct and the process for submitting pull requests.

### Development Workflow

1. Create a feature branch: `git checkout -b feature/your-feature`
2. Make your changes
3. Run tests: `pnpm test`
4. Run linting: `pnpm lint`
5. Commit with semantic commits: `git commit -m "feat: add new feature"`
6. Push to branch: `git push origin feature/your-feature`
7. Open a Pull Request

## 📅 Roadmap

### Phase 1: Foundation (Weeks 1-4)
- [x] Monorepo setup
- [x] Architecture design
- [ ] Authentication system
- [ ] Database schema
- [ ] API foundation

### Phase 2: Core Modules (Weeks 5-12)
- [ ] Battle Board integration
- [ ] Sales Handoff integration
- [ ] Kitchen Manager integration

### Phase 3: Additional Systems (Weeks 13-20)
- [ ] Warehouse module
- [ ] CRM module
- [ ] Scheduling module

### Phase 4: Advanced Features (Weeks 21+)
- [ ] AI integration
- [ ] Analytics & reporting
- [ ] Advanced integrations

## 📞 Support

For support, email support@example.com or open an issue on GitHub.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](./LICENSE) file for details.

## 👥 Team

- **Lead Architecture**: Your Name
- **Development Team**: Team Members

---

**Last Updated**: December 8, 2025
