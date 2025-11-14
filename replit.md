# Spiritual First Aid

## Overview

Spiritual First Aid is a tiered SaaS application designed to equip disaster chaplains, faith leaders, emergency responders, and CPE (Clinical Pastoral Education) programs with evidence-informed spiritual care tools. The platform provides a progressive feature set across three tiers:

- **Free Community**: Core spiritual care frameworks, individual incident tracking, and basic after-action notes
- **Disaster Team ($2.99/month)**: Advanced team collaboration, offline content packs, disaster response protocols, and shared resource libraries
- **CPE Pro ($5.99/month)**: Institutional features including cohort management, supervisor dashboards, competency mapping, and LMS integration

The application emphasizes calm competency, accessibility (WCAG AA+), and offline resilience for crisis situations. It features a spaced-repetition proficiency testing system with 25 core competencies that users must master to achieve certification.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Framework**: React 18 with TypeScript, using Vite as the build tool and development server.

**UI Component System**: Shadcn/ui component library built on Radix UI primitives, configured with the "new-york" style variant. Components follow a design system emphasizing Material Design principles adapted for emergency response contexts.

**Styling Strategy**: Tailwind CSS with extensive CSS custom properties for theming. The design system includes three theme modes (light, dark, and red-light for night operations). Typography uses Inter for UI elements and Merriweather for headings/ritual content to balance functionality with gravitas.

**State Management**: TanStack Query (React Query) for server state management with infinite stale time and disabled auto-refetching to optimize for offline scenarios. Client-side state is handled through React hooks.

**Routing**: Wouter for lightweight client-side routing, chosen for minimal bundle size impact.

**Design Tokens**: Centralized color system using HSL values with CSS variables, supporting both flat (regular buttons) and elevated (cards, popovers) UI patterns. The color palette centers on deep teal (#173B4A) for trust, amber (#D97706) for warmth, and sky blue (#0EA5E9) for alerts.

**Component Architecture**: Feature-based component organization with examples directory for component documentation. Key components include IncidentMode (crisis response interface), ProficiencyTest (spaced-repetition learning), and TeamManagement (collaboration tools).

**Accessibility**: Mobile-first responsive design with minimum 44x44px touch targets, high contrast throughout, and semantic HTML structure.

### Backend Architecture

**Runtime**: Node.js with Express framework, using ESM module format.

**API Design**: RESTful API structure with all endpoints prefixed with `/api/`. Routes handle authentication, incidents, content modules, team management, proficiency testing, and Stripe webhooks.

**Authentication**: Replit Auth integration using OpenID Connect (OIDC) with passport.js strategy. Session management uses connect-pg-simple with PostgreSQL storage for session persistence.

**Authorization**: Tier-based access control middleware (`requireTier`) that restricts features based on user subscription level (free/disaster/pro).

**Data Access Layer**: Centralized storage abstraction in `server/storage.ts` that wraps Drizzle ORM operations, providing a clean interface for data operations across users, teams, incidents, notes, content modules, and proficiency tracking.

**Build Strategy**: Server code is bundled with esbuild for production deployment, maintaining ESM format and external package dependencies.

### Database Architecture

**ORM**: Drizzle ORM with PostgreSQL dialect, using Neon serverless PostgreSQL driver with WebSocket support for connection management.

**Schema Design**: 
- **Users**: Stores Replit auth data, tier information, and Stripe customer/subscription IDs
- **Teams**: Hierarchical team structure with seat limits based on tier
- **Incidents**: Tracks active/closed disaster response cases with JSON-stored checklists
- **Notes**: After-action documentation linked to incidents
- **Content Modules**: Tiered educational content library
- **Resources**: User-curated spiritual care resource directory
- **Questions**: Proficiency test question bank with spaced-repetition metadata
- **User Responses & Proficiency**: Tracks learning progress using SM-2 algorithm for optimal review scheduling
- **Sessions**: PostgreSQL-backed session storage for Replit Auth

**Migration Strategy**: Drizzle Kit for schema migrations with migrations stored in `/migrations` directory.

**Data Validation**: Zod schemas generated from Drizzle table definitions using `drizzle-zod` for runtime type safety and API request validation.

### Key Architectural Decisions

**Spaced Repetition System**: Implements SM-2 algorithm for proficiency testing. Questions are scheduled for retry based on performance, with mastery requiring correct answers at increasing intervals (1 day, 3 days, 7 days, 14 days). This evidence-based approach optimizes long-term retention of critical spiritual care competencies.

**Tier-Based Feature Gating**: Features are unlocked progressively across subscription tiers. Frontend components check user tier and display upgrade prompts for locked content. Backend enforces access control through middleware, preventing API abuse.

**Offline-First Considerations**: While not fully implementing PWA/service workers yet, the architecture prepares for offline support through TanStack Query's stale-time configuration and design system provisions for offline/online indicators.

**Modular Content System**: Content modules are stored with tier associations and tags, allowing flexible categorization and searchability. The system supports future expansion to interactive decision flows and micro-drills.

**Session Security**: Sessions stored in PostgreSQL rather than in-memory stores for horizontal scalability. HTTP-only, secure cookies with 7-day TTL. OIDC token refresh handled automatically.

**Deployment Architecture**: Monolithic deployment with Vite-bundled frontend served by Express in production. Development uses Vite's middleware mode for HMR. Replit-specific plugins provide runtime error overlays and development tooling.

## External Dependencies

### Payment Processing
- **Stripe**: Handles subscription billing for Disaster Team and CPE Pro tiers. Stripe Checkout creates subscription sessions with 7-day trial periods. Webhooks process subscription lifecycle events (created, updated, canceled) to update user tier in database.

### Database
- **Neon Serverless PostgreSQL**: Managed PostgreSQL database accessed via WebSocket connections for Replit compatibility. Connection pooling handled by `@neondatabase/serverless` package.

### Authentication
- **Replit Auth (OIDC)**: Primary authentication provider. Discovery endpoint at `https://replit.com/oidc` with client credentials stored in environment variables. Passport.js strategy handles OAuth flow.

### Third-Party Libraries
- **Radix UI**: Headless component primitives for accessibility-compliant UI elements
- **TanStack Query**: Server state synchronization and caching
- **Drizzle ORM**: Type-safe database access layer
- **date-fns**: Date manipulation for proficiency scheduling
- **zod**: Runtime type validation
- **Tailwind CSS**: Utility-first styling framework

### Development Tools
- **Replit Vite Plugins**: Development banner, cartographer (route visualization), and runtime error modal for enhanced DX on Replit platform
- **TypeScript**: Type safety across client, server, and shared code
- **esbuild**: Fast production bundling for server code

### Environment Variables Required
- `DATABASE_URL`: Neon PostgreSQL connection string
- `SESSION_SECRET`: Session encryption key
- `REPL_ID`: Replit instance identifier for OIDC
- `ISSUER_URL`: OIDC provider URL (defaults to Replit)
- `STRIPE_SECRET_KEY`: Stripe API key for payment processing
- `STRIPE_WEBHOOK_SECRET`: Webhook signature verification
- `STRIPE_PRICE_DISASTER_MONTHLY`: Stripe Price ID for disaster tier monthly
- `STRIPE_PRICE_DISASTER_ANNUAL`: Stripe Price ID for disaster tier annual
- `STRIPE_PRICE_PRO_MONTHLY`: Stripe Price ID for pro tier monthly
- `STRIPE_PRICE_PRO_ANNUAL`: Stripe Price ID for pro tier annual