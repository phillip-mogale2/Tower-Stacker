# Tower Stack Game

## Overview

A 3D tower-stacking game built with React Three Fiber where players tap to drop moving blocks and build the tallest tower possible. The game features physics-based block dropping, score tracking, and audio feedback. Built as a full-stack application with Express.js backend and React frontend using Three.js for 3D rendering.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**React with Three.js 3D Rendering**
- Uses `@react-three/fiber` for declarative 3D scene management in React
- Leverages `@react-three/drei` for helper components and utilities
- Implements `@react-three/postprocessing` for visual effects
- Rationale: React Three Fiber provides a React-friendly API for Three.js, making 3D development more intuitive with component-based architecture
- Alternative considered: Plain Three.js, but R3F offers better React integration and developer experience

**State Management with Zustand**
- Two primary stores: `useGame` (game logic) and `useAudio` (sound management)
- Uses `subscribeWithSelector` middleware for fine-grained state subscriptions
- Rationale: Zustand provides simple, boilerplate-free state management without Context API complexity
- Pros: Minimal setup, good TypeScript support, no providers needed
- Cons: Less ecosystem tooling compared to Redux

**UI Component Library**
- Radix UI primitives for accessible components
- Tailwind CSS for styling with custom design system
- shadcn/ui pattern for component organization
- Rationale: Radix provides unstyled, accessible primitives that work well with Tailwind
- Design tokens defined in CSS variables for theming consistency

**Game Architecture**
- Core game loop runs in `useFrame` hook from R3F
- Block movement, collision detection, and physics handled in game store
- Separation of concerns: rendering (components) vs. logic (stores)
- Rationale: Keeps rendering declarative while centralizing game state

### Backend Architecture

**Express.js Server**
- Minimal REST API setup with `/api` prefix convention
- HTTP server creation with `createServer` for WebSocket support capability
- Custom logging middleware for request/response tracking
- Rationale: Express provides simple, flexible routing with extensive middleware ecosystem

**Static File Serving**
- Vite-built client served from `dist/public` directory
- SPA fallback routing to `index.html` for client-side routing
- Development mode uses Vite middleware for HMR
- Rationale: Separates development and production static serving strategies

**Database Layer Abstraction**
- `IStorage` interface defines CRUD operations
- `MemStorage` provides in-memory implementation
- Designed for easy swap to database implementation
- Rationale: Allows development without database while maintaining production-ready interface
- Note: Drizzle ORM configured for PostgreSQL migration when needed

**Build System**
- esbuild for server bundling with selective dependency bundling
- Vite for client bundling with React and GLSL shader support
- Dependency allowlist strategy reduces syscalls and improves cold starts
- Rationale: esbuild's speed combined with selective bundling optimizes deployment

### Data Storage

**Database Configuration (Drizzle ORM)**
- Configured for PostgreSQL dialect via `@neondatabase/serverless`
- Schema defined in `shared/schema.ts` for type sharing between client/server
- Migration output to `./migrations` directory
- User table with username/password authentication schema
- Rationale: Drizzle provides type-safe SQL queries with zero-cost abstractions
- Neon Serverless driver chosen for serverless deployment compatibility

**Current Implementation**
- In-memory storage with `MemStorage` class
- Maps for quick lookup by ID
- Ready for database swap without code changes beyond storage initialization
- Rationale: Enables rapid prototyping while maintaining production architecture

### Authentication & Authorization

**Planned Authentication**
- User schema supports username/password authentication
- Dependencies include `passport`, `passport-local`, `express-session`
- Session storage configured with `connect-pg-simple` for PostgreSQL
- JWT tokens available via `jsonwebtoken` dependency
- Rationale: Standard session-based auth with optional JWT for API authentication
- Note: Authentication routes not yet implemented

### External Dependencies

**Database Services**
- Neon Serverless PostgreSQL (`@neondatabase/serverless`)
- Connection via `DATABASE_URL` environment variable
- Drizzle ORM for schema management and queries

**3D Graphics & Rendering**
- Three.js (via React Three Fiber)
- GLSL shader support for custom visual effects
- Asset support for GLTF/GLB 3D models and audio files (MP3, OGG, WAV)

**UI Component Libraries**
- Radix UI component primitives (accordion, dialog, dropdown, etc.)
- Lucide React for icons
- class-variance-authority for variant management
- cmdk for command palette functionality

**Development Tools**
- Vite for frontend development and HMR
- TypeScript for type safety
- esbuild for production bundling
- Replit vite plugin for runtime error overlay

**Fonts**
- Inter font via `@fontsource/inter`
- Google Fonts (Poppins, Montserrat) loaded from CDN

**Potential Future Integrations**
- Payment processing via Stripe (dependency present)
- Email via Nodemailer (dependency present)
- AI via OpenAI or Google Generative AI (dependencies present)
- File uploads via Multer (dependency present)
- WebSocket support via `ws` (dependency present)

**Utility Libraries**
- zod for runtime validation
- date-fns for date manipulation
- nanoid for unique ID generation
- axios for HTTP requests