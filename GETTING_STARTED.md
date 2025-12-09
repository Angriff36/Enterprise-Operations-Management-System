# HQ Operations - Getting Started Guide

## Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js**: 18.17 or higher ([Download](https://nodejs.org/))
- **pnpm**: 9.0 or higher ([Install](https://pnpm.io/installation))
- **Git**: Latest version ([Download](https://git-scm.com/))
- **Docker** (optional, for local development): [Download](https://www.docker.com/)

## Initial Setup

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/hq-operations.git
cd hq-operations
```

### 2. Install Dependencies

```bash
pnpm install
```

### 3. Configure Environment Variables

Copy the example environment file and fill in your values:

```bash
cp .env.example .env.local
```

**Required variables**:
- `DATABASE_URL`: Supabase connection string
- `NEXT_PUBLIC_SUPABASE_URL`: Supabase project URL
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`: Supabase anonymous key
- `STRIPE_SECRET_KEY`: Stripe API key
- `RESEND_API_KEY`: Resend email service key
- `AWS_ACCESS_KEY_ID`: AWS S3 access key
- `AWS_SECRET_ACCESS_KEY`: AWS S3 secret key

### 4. Setup Database

```bash
# Run migrations
pnpm db:migrate

# Seed initial data (optional)
pnpm db:seed
```

### 5. Start Development Server

```bash
pnpm dev
```

Visit `http://localhost:3000` to see the application.

## Project Structure

```
hq-operations/
├── apps/
│   ├── web/              # Next.js web application
│   │   ├── app/         # App directory (routes)
│   │   ├── components/  # React components
│   │   ├── hooks/       # Custom React hooks
│   │   ├── lib/         # Utilities and helpers
│   │   └── styles/      # Global styles
│   ├── mobile/           # React Native app
│   └── api/              # Inngest serverless functions
├── packages/
│   ├── database/        # Prisma schema
│   ├── shared/          # Shared types and utilities
│   ├── ui/              # Component library
│   └── config/          # Shared configurations
└── docs/                # Documentation
```

## Available Scripts

### Development

```bash
# Start dev server
pnpm dev

# Start specific app
pnpm dev --filter web
pnpm dev --filter mobile
pnpm dev --filter api
```

### Building

```bash
# Build all apps
pnpm build

# Build specific app
pnpm build --filter web
```

### Testing

```bash
# Run all tests
pnpm test

# Watch mode
pnpm test --watch

# Coverage report
pnpm test --coverage
```

### Code Quality

```bash
# Lint code
pnpm lint

# Fix linting issues
pnpm lint --fix

# Format code
pnpm format

# Type check
pnpm type-check
```

### Database

```bash
# Create migration
pnpm db:migrate:create

# Apply migrations
pnpm db:migrate

# Push schema to database
pnpm db:push

# Seed database
pnpm db:seed

# Open Prisma Studio
pnpm db:studio
```

## Working with Modules

### Battle Board Module

The Battle Board module handles event management and real-time notifications.

**Location**: `apps/web/components/battle-boards/`

**Key Components**:
- `EventBoard.tsx` - Main event display
- `ShiftBoard.tsx` - Shift management
- `NotificationCenter.tsx` - Real-time alerts

### Sales Module

Sales pipeline management and deal tracking.

**Location**: `apps/web/components/sales/`

**Key Components**:
- `SalesHandoff.tsx` - Handoff interface
- `PipelineView.tsx` - Deal pipeline
- `DealTracker.tsx` - Deal management

### Kitchen Module

Kitchen operations and order management.

**Location**: `apps/web/components/kitchen/`

**Key Components**:
- `KitchenBoard.tsx` - Main kitchen display
- `OrderQueue.tsx` - Order management
- `InventoryManager.tsx` - Inventory tracking

## Creating a New Feature

### 1. Create Feature Branch

```bash
git checkout -b feature/your-feature-name
```

### 2. Create Component

```bash
# Add component to appropriate feature folder
apps/web/components/your-module/YourComponent.tsx
```

### 3. Add Types

```bash
# Add types to shared package
packages/shared/src/types/yourTypes.ts
```

### 4. Create API Route (if needed)

```bash
# Add API route
apps/web/app/api/your-endpoint/route.ts
```

### 5. Write Tests

```bash
# Create test file
apps/web/components/your-module/YourComponent.test.tsx
```

### 6. Commit and Push

```bash
git add .
git commit -m "feat: add your feature description"
git push origin feature/your-feature-name
```

## Debugging

### Browser DevTools

- Open Chrome DevTools: `F12`
- React Developer Tools extension recommended
- Check Console for errors and warnings

### VS Code Debugging

Add to `.vscode/launch.json`:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Next.js",
      "type": "node",
      "request": "launch",
      "program": "${workspaceFolder}/node_modules/.bin/next",
      "args": ["dev"],
      "console": "integratedTerminal"
    }
  ]
}
```

### Database Debugging

```bash
# Open Prisma Studio
pnpm db:studio

# View logs
pnpm db:logs
```

## Common Issues

### Port Already in Use

```bash
# Kill process on port 3000
lsof -ti:3000 | xargs kill -9

# Or use different port
pnpm dev --port 3001
```

### Database Connection Error

1. Check `.env.local` has correct DATABASE_URL
2. Ensure database is running
3. Run migrations: `pnpm db:migrate`

### Dependencies Not Installing

```bash
# Clear pnpm cache
pnpm store prune

# Reinstall
pnpm install
```

### TypeScript Errors

```bash
# Regenerate types
pnpm type-check
```

## Next Steps

1. Read the [Architecture Documentation](./ENTERPRISE_ARCHITECTURE.md)
2. Explore the [Architecture Diagrams](./ARCHITECTURE_DIAGRAMS.md)
3. Check the [Contributing Guide](.github/CONTRIBUTING.md)
4. Join the development team

## Getting Help

- **Documentation**: Check README.md and docs folder
- **Issues**: Search existing GitHub issues
- **Discussions**: Start a discussion for questions
- **Email**: Support email (to be configured)

---

Happy coding! 🚀
